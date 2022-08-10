---
title: "How to generate parsiq wallet triggers for CryptoPunks"
date: "November 7, 2021"
excerpt: "We create 3 PARSIQ Platform Smart Triggers that respond to Crypto Punks directly related on-chain events and follow custom logic with parsiq wallet tracking."
cover_image: "/images/posts/parsiq/soc-e1658960455528.webp"
time_read: "5 min"
---

In this post we create three parsiq platform smart Triggers that respond to CryptoPunks directly related on-chain events and follow custom logic to deliver all necessary transaction information about the price action.

Offers for Sale of the limited CryptoPunks collection are particularly vital to traders and collectors. New listings might prove to be an opportunity to profit from the immediate purchase. Meanwhile for traders new sales provide indicators for the demand and supply within the collections as well as for the market, in general.

Cryptopunks is the NFT collection of 10000 uniquely generated characters. The collection has a highest market capitalization and market volume as of November 4th 2021.

![](/images/posts/parsiq/140242896-9c339336-e36b-4388-8aa9-56760bf8cd4a.png)

## Simple Walkthrough

1. We created parsiq account with an Empty project instead of a template as it allows us to edit the trigger code.

![](/images/posts/parsiq/140244531-09e16a47-fa12-40f6-bc77-7d139a8be8f4.png)

2. Next, to monitor the NFT collection we added its contract ABI in the User stream on parsiq platform.

![](/images/posts/parsiq/140627741-bea5116d-0b93-4f4d-af08-6fb4e321223e.png)

We select the ABI straight from the page and export into RAW/TEXT format and upload to parsiq platform.

![](/images/posts/parsiq/140244623-07f2cb8d-c07c-448d-a9d3-809bb6d66600.png)

3. Following data from Etherscan we choose three most frequent events so that we could use them with ParsiQL to create a trigger.

![](/images/posts/parsiq/140629662-b5a2820d-e445-4846-9640-60b68ca4f946.png)

In our instance, we added the events: PunkOffered, PunkBidEntered and PunkBidWithdrawn.

![](/images/posts/parsiq/140629515-5b5ccb36-07ed-46bc-ac06-1293bd50eb78.png)

4. Back in our Project we also included transports to get the necessary data. There are four options: Web, Discord, Telegram and Google Sheets. In this project we implemented Web, Telegram and Google Sheets transports.

![](/images/posts/parsiq/image-768x333.png)

5. As we implemented the necessary details we combine them as a final product. In our Project we create triggers for each of the events with similar ParsiQL code, deploy them and add transports. Here are some details on the code and configuration:

![](/images/posts/parsiq/140609712-a9a03f36-60b8-4e35-b46d-0bede069844d.png)

![](/images/posts/parsiq/140251634-7ab5aeeb-613b-4921-8e32-718ae099d980.png)

![](/images/posts/parsiq/140609790-05521031-d2b7-4904-a67f-3b3a9412eb60.png)

![](/images/posts/parsiq/140245287-348d09c4-b07d-4762-b06a-7034bda3d7bd.png)

## Results

We get immediate notifications through a telegram bot in the private channel.

![](/images/posts/parsiq/140609836-eb44988a-9017-4802-bb92-17279241a2f8.png)

By using ngrok, we receive the post requests from the triggers.

For two out of three trigger we also post the data back on the Platform to update the User Tables with NodeJs.

<div style="background: #f8f8f8; overflow:auto;width:auto;border-width:.1em .1em .1em .8em;padding:.2em .6em;"><pre style="margin: 0; line-height: 125%"><span style="color: #AA22FF; font-weight: bold">var</span> express <span style="color: #666666">=</span> require(<span style="color: #BB4444">&#39;express&#39;</span>)
<span style="color: #AA22FF; font-weight: bold">var</span> request <span style="color: #666666">=</span> require(<span style="color: #BB4444">&#39;request&#39;</span>)

<span style="color: #AA22FF; font-weight: bold">var</span> app <span style="color: #666666">=</span> express()
app.use(express.json())

app.post(<span style="color: #BB4444">&#39;/&#39;</span>, <span style="color: #AA22FF; font-weight: bold">function</span>(req, res) {
console.log(JSON.stringify(req.body));
<span style="color: #AA22FF; font-weight: bold">var</span> options <span style="color: #666666">=</span> {
<span style="color: #BB4444">&#39;method&#39;</span><span style="color: #666666">:</span> <span style="color: #BB4444">&#39;POST&#39;</span>,
<span style="color: #BB4444">&#39;url&#39;</span><span style="color: #666666">:</span> <span style="color: #BB4444">&#39;https://api.parsiq.net/v1/data/{key-of-the-table}&#39;</span>,
<span style="color: #BB4444">&#39;headers&#39;</span><span style="color: #666666">:</span> {
<span style="color: #BB4444">&#39;Authorization&#39;</span><span style="color: #666666">:</span> <span style="color: #BB4444">&#39;Bearer API-key-of-the-project&#39;</span>,
<span style="color: #BB4444">&#39;Content-Type&#39;</span><span style="color: #666666">:</span> <span style="color: #BB4444">&#39;application/json&#39;</span>
},
body<span style="color: #666666">:</span> JSON.stringify([
{
<span style="color: #BB4444">&quot;address&quot;</span><span style="color: #666666">:</span> req.body.fromAddress,
<span style="color: #BB4444">&quot;Punk&quot;</span><span style="color: #666666">:</span> req.body.punkIndex,
<span style="color: #BB4444">&quot;Event&quot;</span><span style="color: #666666">:</span>req.body.event
}
])};

    request(options);
    res.end();

})
<span style="color: #AA22FF; font-weight: bold">const</span> port <span style="color: #666666">=</span> process.env.PORT <span style="color: #666666">||</span> <span style="color: #666666">3000</span>

app.listen(port, () <span style="color: #666666">=&gt;</span> console.log(<span style="border: 1px solid #FF0000">\`Application listening on port ${port} `</span>))

</pre></div>

We also configured three separate Google Sheet spreadsheets that serve as real time database for CryptoPunks collection. We use R to host the database and display the CryptoPunks on-chain activity.

<div style=" position: relative; padding-bottom: 56.25%;">
<iframe style="border: 1; top: 0; left: 0; width: 100%; height: 100%; position: absolute;" src="https://www.youtube.com/embed/5Be_iZKCQd0?autoplay=1&mute=1" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

Further we suggest to utilize the data for a more comprehensive analysis with parsiq.

[GitHub repository for PARSIQ CryptoPunks Offers and Bids Tracking Dashboard](https://github.com/dspytdao/PARSIQ-CryptoPunks)