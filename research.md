---
layout: nosidebar
title: Research
permalink: /research/
---

## Working papers

<hr/>
<details title="mse">
<summary><strong>JMP</strong> <a href="{{ site.baseurl }}/assets/jmp.pdf">The treatment effect elasticity of demand: Estimating the welfare losses from groundwater depletion in India</a><br/>
(2018 Sep) <ar>show abstract</ar>&nbsp;<!--<a href="{{ site.baseurl }}/assets/jmp.pdf">pdf</a>--><hr/></summary>
<br/>I estimate an elasticity of demand for irrigation to its gross returns in rural India. Many approaches to estimating this elasticity fail when agents select into adopting irrigation on heterogeneous gross returns and costs. I develop a novel approach to correct for selection that only requires aggregated data on gross revenue and adoption of irrigation. I use climate and soil characteristics as an instrument for gross returns to irrigation, and hydrogeology as an instrument for irrigation to correct for selection. I estimate a 1% increase in the effect of irrigation on yields causes a 0.7% increase in adoption of irrigation. I use this elasticity to infer changes in profits from changes in adoption of irrigation caused by shocks to its profitability, and to conduct counterfactuals. First, groundwater depletion from 2000-2010 in northwestern India permanently reduced economic surplus by 1.2% of gross agricultural revenue. Second, I evaluate a policy that optimally reduces relative subsidies for groundwater irrigation in districts with large negative pumping externalities, while holding total subsidies fixed. Under the policy, depletion caused by subsidies decreases by 16%, but farmer surplus increases by only 0.07% of gross agricultural revenue.
</details>
<hr style="visibility:hidden;height:12pt" />
<hr/>
<details title="cs">
<summary>Marshallian consumer surplus from intertemporal substitution: Applications to savings, credit, and index insurance<br/>
(2017 Dec) <ar>show abstract</ar><!--include link to paper here, should be href="{{ site.baseurl }}/assets/" with link name "paper"--><hr/></summary>
<br/>The welfare gains from a novel intertemporal substitution technology, such as a new credit or insurance product, are commonly measured using either its effect on a welfare proxy or by estimating a structural model. Using a welfare proxy is often undesirable due to noise in measurement and the challenge of converting estimated effects into a money metric, while structural approaches sometimes require strong functional form assumptions and can be skewed by unexpected moments of the data. In contrast, despite some drawbacks, Marshallian consumer surplus is frequently used as a metric for the welfare gains from access to a new product in a static setting, and with sufficient variation in prices may be relatively easy to precisely estimate. I show that under a broad class of models of dynamic optimization which nest Deaton (1991), Marshallian consumer surplus is a reasonable welfare metric for access to an intertemporal substitution technology. I demonstrate how to calculate it, and apply the approach to three experiments which randomly varied either interest rates or prices: I compare the welfare gains from grants of index insurance in Ghana to their actuarially fair value, I calculate the welfare gains to households from access to a leading MFI in Mexico, and I lower bound the foregone household welfare due to inattention to the Savers' Credit among households in the United States. In all cases, the calculation is straightforward, transparent, and can be represented graphically as a ``welfare triangle''.
</details><br/>

## Work in progress
<hr/><summary>Irrigation in Rwanda: Farmer responses to massive increases in the production possibilities frontier (with Maria Jones, Florence Kondylis, and Jeremy Magruder) <hr/></summary>

<script>
function getQueryVariable(variable)
{
       var query = window.location.search.substring(1);
       var vars = query.split("&");
       for (var i=0;i<vars.length;i++) {
               var pair = vars[i].split("=");
               if(pair[0] == variable){return pair[1];}
       }
       return(false);
}
var whichDetailsOpen = getQueryVariable("open");
var detailsCollection = document.getElementsByTagName("details");
function matchDetailsTitle()
{
       for (var i=0;i<detailsCollection.length;i++) {
               var detailsTitle = detailsCollection[i].getAttribute("title");
               if(detailsTitle == whichDetailsOpen){return i;}
       }
       return(-1)
}
var detailsTitle = matchDetailsTitle();
if(detailsTitle+1)
{
document.getElementsByTagName("details")[detailsTitle].setAttribute("open", "open");
//document.getElementsByTagName("details")[detailsTitle].scrollIntoView();
}
</script>


