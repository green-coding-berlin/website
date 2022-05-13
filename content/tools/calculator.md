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
	function calculateCO2() {
		const sizeSize = document.getElementById("sitesize").value;
		const pageviews = document.getElementById("pageviews").value;
		const imagesSize = document.getElementById("imagessize").value;
		const mbTokWh = 0.0023;
		const kWhToCO2 = 0.519;
		const compression = 0.3;
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
		const backlash =
			pageviews * 0.0346 * 82 * 1 + (pageviews / 2) * 0.0346 * 82 * 11;
		const backlashCO2 = (backlash * mbTokWh * kWhToCO2).toFixed(2);

		document.getElementById("answer1").innerText =
			websiteCO2 + " kilos of carbon per year";
		document.getElementById("answer2").innerText =
			webpCO2 +
			" kilos of carbon would be saved per year with a typical 30% compression rate of webP but as backlash you would possibly emit ~" +
			backlashCO2 +
			"kg CO2 per year for the download of newer browsers that could benefit from the webP format, making it a carbon-" +
			(backlashCO2 - webpCO2 > 0 ? "unfriendly" : "friendly") +
			" decision";
	}
</script>

<style>
	.accordion {
		background-color: #eee;
		color: #444;
		cursor: pointer;
		padding: 18px;
		width: 100%;
		border: none;
		text-align: left;
		outline: none;
		font-size: 15px;
		transition: 0.4s;
	}

	.active,
	.accordion:hover {
		background-color: #ccc;
	}

	.panel {
		padding: 0 18px;
		display: none;
		background-color: white;
		overflow: hidden;
	}
</style>

<div id="calculator" class="container bg-two">
	<div id="ancla6"></div>
	<div class="section-two">
		<div class="title-two">Our CO2 calculator</div>
		<div class="separator"><div class="line line-2"></div></div>
		<div class="data-content-two">This is our calculator</div>
		<form id="formulario" action="/" method="post">
			<fieldset>
				<div class="first">
					<label class="data-form" for="sitesize"
						>Website size in megabytes</label
					>
					<input
						type="number"
						name="sitesize"
						id="sitesize"
						value="1"
					/>
					<label class="data-form" for="pageviews"
						>Pageviews per month</label
					>
					<input
						type="number"
						name="pageviews"
						id="pageviews"
						value="1000"
					/>
					<label class="data-form" for="answer1"
						>Estimated carbon emitted per year:</label
					>
					<p id="answer1" style="color: white"></p>

					<label class="data-form" for="images"
						>Uncompressed images on server (in total MB)</label
					>
					<input
						type="number"
						name="imagessize"
						id="imagessize"
						value="0.5"
					/>
					<p id="answer2" style="color: white"></p>
				</div>
				<p>
					<a href="#" class="submit" onclick="calculateCO2()"
						>Calculate</a
					>
				</p>
			</fieldset>
		</form>

		<button class="accordion">How we calculate the emitted CO2</button>

		<div class="panel">
			<p>
				To estimate the kWh, we multiply the total megabytes by 0.0023.
				To convert to carbon, we multiply by 0.519 to get kilograms of
				carbon. Using this model, we estimate transmitting 1000 MB would
				result in 1000 ✕ 0.0023 ✕ 0.519 = 1.1937 kilos of carbon
				emitted. Assuming 1000 pageviews per month (as an example), in a
				year time the total emitted carbon would be 1.1937 x 1000 x 12 =
				14324.4kg CO2.
			</p>
		</div>

		<button class="accordion">System boundaries</button>

		<div class="panel">
			<p>
				We represent the system boundary including the Internet Protocol
				(IP) core network and access networks only, which we refer to as
				the “transmission network.” This system boundary was chosen as
				it represents the network of equipment used for data
				transmission and access at a national level. The electricity
				intensity of the transmission network is independent of the data
				type; for example, media streaming, financial transactions,
				e-mail, etc. The electricity intensity of user devices and data
				centers is highly variable, depending largely on the service
				being provided. These subsystems, together with home/on-site
				networking equipment, also tend to have low utilization and high
				“fixed” electricity use, making estimates sensitive to
				assumptions on usage and the allocation method used. This
				approach follows the argument of Coroama and colleagues (2015),
				who suggest assessing user devices and data centers separately
				to the transmission network “and to add them up when needed—for
				example, for the assessment of the energy needs of a specific
				service” (Coroama et al. 2015, 12).
			</p>
		</div>

		<button class="accordion">Embodied carbon</button>

		<div class="panel">
			<p>
				The device you are using to access this website released some
				carbon in its creation; once it reaches the end of life,
				disposing of it may release more. Embodied carbon is the amount
				of carbon pollution emitted during the creation and disposal of
				a device. When calculating the total carbon pollution for the
				computers running your software, account for both the carbon
				pollution to run the computer and the computer's embodied
				carbon. The embodied carbon cost is often much higher for
				consumer devices, sometimes more significant than the lifetime
				carbon cost from electricity consumption (Belkhir and Elmeligi,
				2017). According to Malmodin and Lundén (2018), the average
				embodied carbon footprint of some key user devices are:
			</p>
			<ul>
				<li>Desktop: <span style="color: red">~400kg CO2</span></li>
				<li>Laptop: <span style="color: red">~200kg CO2</span></li>
				<li>Smartphones: <span style="color: red">~60kg CO2</span></li>
			</ul>
		</div>

		<button class="accordion">Backlashes</button>

		<div class="panel">
			<p>
				According to caniuse.com, webP format is not supported by
				Internet Explorer and by (very) old versions of a few other
				browsers. In total, the market share (May 2022) of all those
				browsers combined is 3.46% (0.14 + 0.57 + 0.12 + 0.6 + 0.03 +
				0.62 + 0 + 1.31 + 0 + 0.07). This might differ from your
				userbase's browser usage statistics, but let's stick to this
				number for simplicity.
			</p>

			<p>Two possible backlash scenarios:</p>
			<ul>
				<li>
					3.46% (at most) of the website users would have to download
					and install a different browser to be able to see the webP
					images. As of May 2022, a Chrome desktop installer for
					Windows has ~82MB in size. Assuming 1000 unique visitors per
					month, and a rate of 50% returning visitors, 3.46% of 1000
					users (34.6 users) in the first month, and subsequentially
					half that amount (17,3 users) would need to download 82MB
					per month => (34.6 * 82 * 1) + (17.3 * 82 * 11) = 2837.2 +
					15604.6 MB = 18441.8 MB per year =
					<span style="color: red">22.01kg CO2 per year</span> as
					backlash.
				</li>
				<li>
					In an extreme scenario, think of an app that gets updated
					and only runs correctly on iOS devices. The current (May
					2022) mobile operating system market share of iOS devices
					accounts for 27.68%. In this edge case, up to 72.32% of your
					app users would need to buy iPhones in order to use your
					app. Considering 1000 unique users per year and 60kg of
					embodied carbon for an average phone, 723.2 users * 60kg CO2
					=
					<span style="color: red">43392kg CO2 per year</span> would
					be emitted per year as backlash of your app update.
				</li>
			</ul>
		</div>

		<button class="accordion">Sources</button>

		<div class="panel">
			<p>Emitted CO2:</p>
			<ul>
				<li>
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
					<a
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
						>Malmodin, J.; Lundén, D. The Energy and Carbon
						Footprint of the Global ICT and E&M Sectors 2010–2015.
						Sustainability 2018, 10, 3027</a
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

			<p>Backlashes:</p>
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
	</div>
</div>
<!-- end calculator -->

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
