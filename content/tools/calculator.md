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
			`<span>Per year:</span>
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
		if (id === "button1") {
			document.getElementById("calc1").style.display = "block";
			document.getElementById("calc2").style.display = "none";
			document.getElementById("calc3").style.display = "none";
		} else if (id === "button2") {
			document.getElementById("calc1").style.display = "none";
			document.getElementById("calc2").style.display = "block";
			document.getElementById("calc3").style.display = "none";
		} else if (id === "button3") {
			document.getElementById("calc1").style.display = "none";
			document.getElementById("calc2").style.display = "none";
			document.getElementById("calc3").style.display = "block";
		}
	};
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

	.submit {
		padding: 7px;
		margin: 15px;
	}

	.calc {
		display: flex;
		flex-direction: column;
		align-items: center;
	}

	.form-input {
		display: flex;
		margin-bottom: 5px;
	}

	.form-input > input {
		width: 4em;
		margin-left: 10px;
	}

	.form-input > label {
		font-size: 0.9em;
		text-transform: none;
	}

	.form-answer {
		margin-top: 10px;
		display: flex;
		flex-direction: column;
	}

	.form-answer ul {
		background: #ffffff;
		padding: 5px;
	}

	.form-answer ul li {
		background: #3d936f;
		color: white;
		margin: 0px;
		padding: 5px;
		list-style-type: none;
	}

	.form-answer p {
		font-size: 0.9em;
	}

	.form-answer span {
		color: black;
		font-weight: bold;
	}

	/* .calc-div {
		display: flex;
		flex-direction: column;
		align-items: center;
	} */

	.buttons-div {
		display: flex;
		justify-content: center;
		padding: 10px;
	}

	.buttons-div button {
		appearance: none;
		background-color: #2ea44f;
		border: 1px solid rgba(27, 31, 35, 0.15);
		border-radius: 6px;
		box-shadow: rgba(27, 31, 35, 0.1) 0 1px 0;
		box-sizing: border-box;
		color: #fff;
		cursor: pointer;
		display: inline-block;
		font-family: -apple-system, system-ui, "Segoe UI", Helvetica, Arial,
			sans-serif, "Apple Color Emoji", "Segoe UI Emoji";
		font-size: 0.8em;
		line-height: 20px;
		padding: 6px 16px;
		position: relative;
		text-align: center;
		text-decoration: none;
		user-select: none;
		-webkit-user-select: none;
		touch-action: manipulation;
		vertical-align: middle;
		white-space: nowrap;
	}

	.buttons-div button:focus:not(:focus-visible):not(.focus-visible) {
		box-shadow: none;
		outline: none;
	}

	.buttons-div button:hover {
		background-color: #2c974b;
	}

	.buttons-div button:focus {
		box-shadow: rgba(46, 164, 79, 0.4) 0 0 0 3px;
		outline: none;
	}

	.buttons-div button:disabled {
		background-color: #94d3a2;
		border-color: rgba(27, 31, 35, 0.1);
		color: rgba(255, 255, 255, 0.8);
		cursor: default;
	}

	.buttons-div button:active {
		background-color: #298e46;
		box-shadow: rgba(20, 70, 32, 0.2) 0 1px 0 inset;
	}
</style>

<div id="calculator" class="container bg-two">
	<div class="buttons-div">
		<button id="button1" onclick="toggleTab(this.id)">webP</button>
		<button id="button2" onclick="toggleTab(this.id)">gzip</button>
		<button id="button3" onclick="toggleTab(this.id)">Calc3</button>
	</div>

	<div class="calc" id="calc1">
		<h4>webP Calculator</h4>
		<form id="formulario" action="/" method="post">
			<fieldset>
				<div class="calc-div">
					<div class="form-input">
						<label class="data-form" for="sitesize"
							>Website size (in MB):</label
						>
						<input
							type="number"
							name="sitesize"
							id="sitesize"
							value="5"
						/>
					</div>

					<div class="form-input">
						<label class="data-form" for="pageviews"
							>Pageviews per month:</label
						>
						<input
							type="number"
							name="pageviews"
							id="pageviews"
							value="1000"
						/>
					</div>

					<div class="form-input">
						<label class="data-form" for="images"
							>Uncompressed images on server (in MB):</label
						>
						<input
							type="number"
							name="imagessize"
							id="imagessize"
							value="2"
						/>
					</div>

					<div class="form-input">
						<label class="data-form" for="compression"
							>webP compression rate (in %):</label
						>
						<input
							type="number"
							name="compression"
							id="compression"
							value="30"
						/>
					</div>

					<div class="form-answer">
						<p id="answer" style="color: white"></p>
						<a
							href="javascript:void(null);"
							class="submit"
							onclick="calculateCO2()"
							>Calculate</a
						>
					</div>
				</div>
			</fieldset>
		</form>
	</div>

	<div class="calc" id="calc2" style="display: none">
		<h3>gzip Calculator</h3>
	</div>

	<div class="calc" id="calc3" style="display: none">
		<h3>3rd Calculator</h3>
	</div>

	<button class="accordion">How we calculate the emitted CO2</button>

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

	<button class="accordion">System boundaries</button>

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

	<button class="accordion">Embodied carbon</button>

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

	<button class="accordion">Rebound Effect</button>

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

	<button class="accordion">Sources</button>

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

		<p>Rebout Effect:</p>
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
