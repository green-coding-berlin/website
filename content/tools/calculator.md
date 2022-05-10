---
title: "Calculator"
date: 2022-05-09
draft: false
author: "Alexandre Oliveira"
authorlink: "https://www.linkedin.com/in/alexandre-oliv"
summary: "Our CO2 calculator"
---

{{< rawhtml >}}

<script>
function calculateCO2() {
  document.getElementById("answer").innerText = (document.getElementById("megabytesform").value * 0.0023 * 0.519) + " kilos of carbon";
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

.active, .accordion:hover {
  background-color: #ccc; 
}

.panel {
  padding: 0 18px;
  display: none;
  background-color: white;
  overflow: hidden;
}
</style>

<div id="calculator" class="container bg-two"><div id="ancla6"></div> 
        <div class="section-two">
          <div class="title-two">Our CO2 calculator</div>
          <div class="separator"><div class="line line-2"></div></div>
            <div class="data-content-two">
            	This is our calculator
            </div>
            <form id="formulario" action="/" method="post">	
            <fieldset>
            <div class="first">					
              
          
<label class="data-form" for="megabytesform">Megabytes</label>
<input type="text" name="megabytesform" id="megabytesform" value="0" onclick="if(this.value=='0') this.value=''" onblur="if(this.value=='') this.placeholder='0'">
<label class="data-form" for="answer">Estimated carbon emitted:</label><p id="answer"></p>
</div>
<p><a href="#" class="submit" onclick="calculateCO2()">Calculate</a></p>
</fieldset>
</form>

<button class="accordion">How we calculate the emitted CO2</button>

<div class="panel">
  <p>To estimate the kWh, we multiply the total megabytes by 0.0023. To convert to carbon, we multiply by 0.519 to get kilograms of carbon. Using this model, we estimate transmitting 1000 MB would result in 1000 ✕ 0.0023 ✕ 0.519 = 1.1937 kilos of carbon emitted.</p>
</div>

<button class="accordion">System boundaries</button>

<div class="panel">
  <p>We grouped the results by Internet subsystem (according to definitions in table 1), to evaluate the impact of differing system boundaries on variability of estimates. Across the 14 studies, estimates were derived from eight different combinations of subsystems. We therefore recalculated estimates to represent a common system boundary (see figure 1), including the Internet Protocol (IP) core network and access networks only, which we refer to as the “transmission network.” This system boundary was chosen as it represents the network of equipment used for data transmission and access at a national level. The electricity intensity of the transmission network is independent of the data type; for example, media streaming, financial transactions, e-mail, etc. The electricity intensity of user devices and data centers is highly variable, depending largely on the service being provided (Coroama et al. 2015). These subsystems, together with home/on-site networking equipment, also tend to have low utilization and high “fixed” electricity use, making estimates sensitive to assumptions on usage and the allocation method used. This approach follows the argument of Coroama and colleagues (2015), who suggest assessing user devices and data centers separately to the transmission network “and to add them up when needed—for example, for the assessment of the energy needs of a specific service” (Coroama et al. 2015, 12).</p>
</div>

<button class="accordion">Sources</button>

<div class="panel">
  <p>Emitted CO2: <a href="https://docs.microsoft.com/en-gb/learn/modules/sustainable-software-engineering-overview/8-network-efficiency" target="_blank">The Principles of Sustainable Software Engineering</a></p>

  <p>System boundaries: <a href="https://onlinelibrary.wiley.com/doi/full/10.1111/jiec.12630" target="_blank">Electricity Intensity of Internet Data Transmission: Untangling the Estimates</a></p>
</div>

</div>

</div><!-- end calculator -->

<script>
var acc = document.getElementsByClassName("accordion");
var i;

for (i = 0; i < acc.length; i++) {
  acc[i].addEventListener("click", function() {
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
