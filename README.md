# Instaplot
a decentralized SaaS for generating [Chia](https://github.com/Chia-Network/chia-blockchain) plots on the [Akash network](https://github.com/ovrclk/akash)

# Why
I think these two technologies complement each other very well. 

Chia is a blockchain based on Proof of Space and Time. You farm it by first generating "plots". Then you store these plots on your hard drive and run the farming software to win coins (which is basically a lottery ticket until pooling is available). Although farming plots is very easy and lightweight, filling your hard drive with the plots can be a very intensive process. To optimize potential rewards, you need to fill your drives quickly so that you have more plots than anyone else. To fill your drives quickly, you need:
1. The hardware to generate the plot files faster.
2. Multiple machines so you can plot in parallel.
3. To spend as little as possible in order to increase the chances that you'll make a return on your investment.

You could buy the hardware outright, but that's a huge investment that not everyone can or should make.

The big cloud platforms (AWS, GCP) will have the hardware you want, enough of them, and the prices look ok on the surface. The hidden cost here is bandwidth, which may cost you 5-10 dollars alone. 

You could purchase from another plot generating SaaS. Usually that would be run by someone who has made a large investment in hardware, but they may still not have enough to plot many plots in parallel. 

Akash network is a decentralized cloud platform. Anyone can provide their own hardware to the network and receive income. **In theory**, it should have hardware you want in large quantity and at affordable prices (currently they aren't charging for bandwidth at least). We'll soon figure out if that's currently the case, or at least drive some more adoption. 😁

# Architecture 
The basic idea here is to have a server for the point of sale stuff, then spin up worker servers to do the hard work of plotting. The point of sale server can be very small and run constantly. Worker servers will need significantly more resources so it's best to only keep them for the short period of time that they are working + some time for the user to download the file. 

NOTE: Akash doesn't offer any storage volumes. I had to compensate for that by combining a ton of stuff all in one docker image. Instead of writing a dockerfile, I just installed the stuff by hand and committed that to build the image. So that's why some docker images don't have docker files

## Instaplot
TODO

## Autoplotter
The autoplotter_base image contains nginx, the MadMax plotter for plotting, chiapos for verification, npm and nodejs. The autoplotter image can be seen in the dockerfile. Emails are sent using Postmark in nodejs
This server generates a single plot, verifies that the plot is valid, and sends an email with the link to the file after it's successfully completed generating. It is able to restart the plotting process if the plot fails to generate or if the plot ends up being invalid. It will only re-attempt once. 

Coming soon: Uploading plots directly to Sia Skynet after generation. 


# Installation
1. First you will need an Akash wallet and some tokens. 
2. You will need your own Postmark account and API token for the emails. Replace the environment variables in the Instaplot deploy.yaml with your own
3. To make deployment easier, use [this UI for interacting with Akash](https://github.com/tombeynon/akash-deploy)
4. Create a new deployment
5. Copy and paste the deploy.yaml content into the text area
6. Click the submit button. Assuming you've got enough tokens, it should continue without error
7. You should hopefully receive bids on your deployment. Choose one to accept and create a lease
8. The application should now be live. 
9. **You'll really need to monitor your Akash wallet**. Your wallet will be drained when someone purchases a plot. If you don't have enough, you could end up charging the person and they won't receive their plots. You also need to monitor the Instaplot UI deployment now and then. Top up the funds using the UI to ensure that the website is up for people to pay for plots.
