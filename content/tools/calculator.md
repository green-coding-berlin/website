---
title: "Calculator"
date: 2022-05-12
draft: false
author: "Alexandre Oliveira"
authorlink: "https://www.linkedin.com/in/alexandre-oliv"
summary: "Our CO2 calculator"
---

{{< rawhtml >}}

<script>
	calculateCO2 = () => {
		const sizeSize = document.getElementById("sitesize").value;
		const pageviews = document.getElementById("pageviews").value;
		const imagesSize = document.getElementById("imagessize").value;
		const compression = document.getElementById("compression").value / 100;
		const mbTokWh = 0.0023;
		const kWhToCO2 = 0.519;
		const months = 12;
		const websiteCO2 = (
			sizeSize *
			pageviews *
			mbTokWh *
			kWhToCO2 *
			months
		).toFixed(2);
		const webpCO2 = (
			imagesSize *
			compression *
			pageviews *
			mbTokWh *
			kWhToCO2 *
			months
		).toFixed(2);
		const rebound =
			pageviews * 0.0346 * 82 * 1 + (pageviews / 2) * 0.0346 * 82 * 11;
		const reboundCO2 = (rebound * mbTokWh * kWhToCO2).toFixed(2);

		document.getElementById("answer").innerHTML =
			`Per year:
			<ul>
				<li>Carbon emitted (before webP compression): <span>` +
			websiteCO2 +
			`kg</span></li>
				<li>webP savings: <span>` +
			webpCO2 +
			`kg</span> (` +
			Math.round((webpCO2 / websiteCO2) * 100) +
			`%)</li>
			<li>Rebound effect: <span>` +
			reboundCO2 +
			`kg</span></li>
			<li><span style="text-decoration: underline">Carbon-` +
			(reboundCO2 - webpCO2 > 0 ? "unfriendly" : "friendly") +
			`</span> decision</li>
			</ul>`;
	};

	toggleTab = (id) => {
		if (id === "show-webp") {
			document.getElementById("webp-calculator").style.display = "block";
			document.getElementById("gzip-calculator").style.display = "none";
			document.getElementById("cloudflare-calculator").style.display = "none";
		} else if (id === "show-gzip") {
			document.getElementById("webp-calculator").style.display = "none";
			document.getElementById("gzip-calculator").style.display = "block";
			document.getElementById("cloudflare-calculator").style.display = "none";
		} else if (id === "show-cloudflare") {
			document.getElementById("webp-calculator").style.display = "none";
			document.getElementById("gzip-calculator").style.display = "none";
			document.getElementById("cloudflare-calculator").style.display = "block";
		}
	};
</script>

<style>

	.panel {
		padding: 0 18px;
		display: none;
        color: white;
	}


    .calc-formfield{
        border-radius: 8px;
        border: none;
        background: #3fb79f;
        width: 50%;
        height: 13px;
        font-family: 'Open Sans', sans-serif;
        font-size: 13px;
        font-weight: normal;
        color: #ffffff;
        padding: 15px 20px;
        outline: none;
        margin: 12px 0 20px;
    }

	#answer span {
		color: black;
		font-weight: bold;
	}

</style>

<div id="filters" class="option-set clearfix foliomenu" style="padding-top: 20px;">
  <a id="show-webp" href="#" onclick="toggleTab(this.id); return false;" class="folio-btn"><div class="portfolio-btn">webP</div></a>
  <a id="show-gzip" href="#" onclick="toggleTab(this.id); return false;" class="folio-btn"><div class="portfolio-btn">gzip</div></a>
  <a id="show-cloudflare" href="#" onclick="toggleTab(this.id); return false;" class="folio-btn"><div class="portfolio-btn">Cloudlfare</div></a>
</div>
<div class="calc" id="webp-calculator" style="padding: 20px;">
    <div class="title-two">webP Calculator</div>
    <form action="/" method="post">
         <div class="container">
             <div class="eight columns"> <!-- Labels -->
                <label class="data-form" for="sitesize">Website size (in MB):</label>
             </div>
             <div class="eight columns"> <!-- Inputs -->
                <input type="number" name="sitesize" class="calc-formfield" id="sitesize" value="5">
             </div>
             <div class="eight columns"> <!-- Labels -->
                <label class="data-form" for="pageviews">Pageviews per month:</label>
             </div>
             <div class="eight columns"> <!-- Inputs -->
                <input type="number" name="pageviews" id="pageviews" class="calc-formfield" value="1000">
             </div>
             <div class="eight columns"> <!-- Labels -->
                <label class="data-form" for="images">Uncompressed images on server (in MB):</label>
             </div>
             <div class="eight columns"> <!-- Inputs -->
                <input type="number" name="imagessize" id="imagessize" class="calc-formfield" value="2">
             </div>
             <div class="eight columns"> <!-- Labels -->
                <label class="data-form" for="compression">webP compression rate (in %):</label>
             </div>
             <div class="eight columns"> <!-- Inputs -->
                <input type="number" name="compression" id="compression" class="calc-formfield" value="30">
             </div>
             <div class="eight columns"> <!-- Inputs -->
                 &nbsp;
             </div>
             <div class="eight columns form-answer">
                <p id="answer" style="color: white"></p>
                <a style="float:left; line-height: 20px;" href="#" class="submit" onclick="calculateCO2(); return false;" >Calculate</a>
            </div>
        </div>
    </form>
</div><!-- End of webP calculator -->
<div class="calc" id="gzip-calculator" style="display: none;">
    <h3>gzip Calculator</h3>
</div>
<div class="calc" id="cloudflare-calculator" style="display: none;">
    <h3>Cloudflare Calculator</h3>
</div>

<div class="line line-1" style="margin: 25px auto;"></div>

<div class="title-two">Explanations / Sources</div>

<a class="folio-btn accordion" style="width: 70%;"><div class="portfolio-btn" style="float:none;">webP</div></a>

<div class="panel">
    <p>
        To estimate the kWh, we multiply the total megabytes by
        0.0023<sup>[1]</sup>. To convert to carbon, we multiply by 0.519<sup
            >[1]</sup
        >
        to get kilograms of carbon. Using this model, we estimate
        transmitting 1000 MB would result in 1000 ✕ 0.0023 ✕ 0.519 = 1.1937
        kilos of carbon emitted. Assuming 1000 pageviews per month (as an
        example), in a year time the total emitted carbon would be 1.1937 x
        1000 x 12 = 14324.4kg CO2.
    </p>
</div>

<a class="folio-btn accordion" style="width: 70%;"><div class="portfolio-btn" style="float:none;">System boundaries</div></a>

<div class="panel">
    <p>
        We represent the system boundary including the Internet Protocol
        (IP) core network and access networks only, which we refer to as the
        “transmission network.”<sup>[2]</sup> This system boundary was
        chosen as it represents the network of equipment used for data
        transmission and access at a national level. The electricity
        intensity of the transmission network is independent of the data
        type; for example, media streaming, financial transactions, e-mail,
        etc. The electricity intensity of user devices and data centers is
        highly variable, depending largely on the service being provided.
        These subsystems, together with home/on-site networking equipment,
        also tend to have low utilization and high “fixed” electricity use,
        making estimates sensitive to assumptions on usage and the
        allocation method used.
    </p>
    <img
        src="/img/calculator/system-boundaries.webp"
        alt="System boundaries"
        width="600"
    />
</div>

<a class="folio-btn accordion" style="width: 70%;"><div class="portfolio-btn" style="float:none;">Embodied Carbon</div></a>

<div class="panel">
    <p>
        The device you are using to access this website released some carbon
        in its creation; once it reaches the end of life, disposing of it
        may release more. Embodied carbon is the amount of carbon pollution
        emitted during the creation and disposal of a device. When
        calculating the total carbon pollution for the computers running
        your software, account for both the carbon pollution to run the
        computer and the computer's embodied carbon. The embodied carbon
        cost is often much higher for consumer devices, sometimes more
        significant than the lifetime carbon cost from electricity
        consumption (Belkhir and Elmeligi, 2017). According to Malmodin and
        Lundén (2018), the average embodied carbon footprint of some key
        user devices are:
    </p>
    <ul>
        <li>Desktop: <span style="color: red">~400kg CO2</span></li>
        <li>Laptop: <span style="color: red">~200kg CO2</span></li>
        <li>Smartphones: <span style="color: red">~60kg CO2</span></li>
    </ul>
</div>

<a class="folio-btn accordion" style="width: 70%;"><div class="portfolio-btn" style="float:none;">Rebound</div></a>

<div class="panel">
    <p>
        According to caniuse.com, webP format is not supported by Internet
        Explorer and by (very) old versions of a few other browsers. In
        total, the market share (May 2022) of all those browsers combined is
        3.46% (0.14 + 0.57 + 0.12 + 0.6 + 0.03 + 0.62 + 0 + 1.31 + 0 +
        0.07). This might differ from your userbase's browser usage
        statistics, but let's stick to this number for simplicity.
    </p>

    <p>Two possible rebound scenarios:</p>
    <ul>
        <li>
            3.46% (at most) of the website users would have to download and
            install a different browser to be able to see the webP images.
            As of May 2022, a Chrome desktop installer for Windows has ~82MB
            in size. Assuming 1000 unique visitors per month, and a rate of
            50% returning visitors, 3.46% of 1000 users (34.6 users) in the
            first month, and subsequentially half that amount (17,3 users)
            would need to download 82MB per month => (34.6 * 82 * 1) + (17.3
            * 82 * 11) = 2837.2 + 15604.6 MB = 18441.8 MB per year =
            <span style="color: red">22.01kg CO2 per year</span> as rebound.
        </li>
        <li>
            In an extreme scenario, think of an app that gets updated and
            only runs correctly on iOS devices. The current (May 2022)
            mobile operating system market share of iOS devices accounts for
            27.68%. In this edge case, up to 72.32% of your app users would
            need to buy iPhones in order to use your app. Considering 1000
            unique users per year and 60kg of embodied carbon for an average
            phone, 723.2 users * 60kg CO2 =
            <span style="color: red">43392kg CO2 per year</span> would be
            emitted per year as rebound of your app update.
        </li>
    </ul>

</div>

<a class="folio-btn accordion" style="width: 70%;"><div class="portfolio-btn" style="float:none;">Sources</div></a>

<div class="panel">
    <p>Emitted CO2:</p>
    <ul>
        <li>
            1.
            <a
                href="https://docs.microsoft.com/en-gb/learn/modules/sustainable-software-engineering-overview/8-network-efficiency"
                target="_blank"
                rel="noopener noreferrer"
                >The Principles of Sustainable Software Engineering</a
            >
        </li>
    </ul>

    <p>System boundaries:</p>
    <ul>
        <li>
            2. <a
                href="https://onlinelibrary.wiley.com/doi/full/10.1111/jiec.12630"
                target="_blank"
                rel="noopener noreferrer"
                >Electricity Intensity of Internet Data Transmission:
                Untangling the Estimates</a
            >
        </li>
    </ul>

    <p>Embodied carbon:</p>
    <ul>
        <li>
            <a
                href="https://docs.microsoft.com/en-gb/learn/modules/sustainable-software-engineering-overview/6-embodied-carbon"
                target="_blank"
                rel="noopener noreferrer"
                >Principle 4: Embodied carbon</a
            >
        </li>
        <li>
            <a
                href="https://doi.org/10.3390/su10093027"
                target="_blank"
                rel="noopener noreferrer"
                >Malmodin, J.; Lundén, D. The Energy and Carbon Footprint of
                the Global ICT and E&M Sectors 2010–2015. Sustainability
                2018, 10, 3027</a
            >
        </li>
        <li>
            <a
                href="https://www.fastcompany.com/90165365/smartphones-are-wrecking-the-planet-faster-than-anyone-expected"
                target="_blank"
                rel="noopener noreferrer"
                >Smartphones Are Killing The Planet Faster Than Anyone
                Expected</a
            >
        </li>
    </ul>

    <p>Rebound Effect:</p>
    <ul>
        <li>
            <a
                href="https://caniuse.com/webp"
                target="_blank"
                rel="noopener noreferrer"
                >Can I use webP?</a
            >
        </li>
        <li>
            <a
                href="https://gs.statcounter.com/os-market-share/mobile/worldwide"
                target="_blank"
                rel="noopener noreferrer"
                >Mobile Operating System Market Share Worldwide</a
            >
        </li>
    </ul>

</div>

<script>
	var acc = document.getElementsByClassName("accordion");
	var i;

	for (i = 0; i < acc.length; i++) {
		acc[i].addEventListener("click", function () {
			this.classList.toggle("active");
			var panel = this.nextElementSibling;
			if (panel.style.display === "block") {
				panel.style.display = "none";
			} else {
				panel.style.display = "block";
			}
		});
	}
</script>

{{< /rawhtml >}}
