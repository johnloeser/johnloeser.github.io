---
layout: nosidebar
title: Research
permalink: /research/
---

## Work in progress

<hr/>
<details title="mwe">
<summary><strong>JMP</strong> The treatment effect elasticity of demand: Estimating the welfare losses from groundwater depletion in India<br/>
(2018 Sep) <ar>show abstract</ar><hr/></summary>
<br/>This paper defines and estimates the treatment effect elasticity of demand: a 1% increase in the effect of irrigation on gross revenue causes a 0.7% increase in the irrigated share of agricultural land in India. In defining this elasticity, this paper enables the extension of conventional revealed preference approaches to settings without prices. I show that this elasticity is the ratio of treatment on the treated to the difference between two linear instrumental variable estimators (in my context, instrumenting for the effect of irrigation on gross revenue). The first uses an (invalid) instrument for potential outcome under treatment (in my context, climate and soil as an instrument for gross revenue under irrigation). The second uses a conventional instrument for treatment (in my context, hydrogeology as an instrument for irrigation). I use the estimated elasticity to conduct welfare counterfactuals. First, groundwater depletion from 2000-2010 in northwestern India permanently reduced economic surplus by 1.2% of gross agricultural revenue. Second, optimally reducing relative subsidies for groundwater irrigation in districts with large negative pumping externalities, while holding total subsidies fixed, would increase farmer surplus by 0.15% of gross agricultural revenue.
</details>
<hr style="visibility:hidden;height:12pt" />
<hr/>
<details title="cs">
<summary>Marshallian consumer surplus from intertemporal substitution: Applications to savings, credit, and index insurance<br/>
(2017 Dec) <ar>show abstract</ar><!--include link to paper here, should be href="{{ site.baseurl }}/assets/" with link name "paper"--><hr/></summary>
<br/>The welfare gains from a novel intertemporal substitution technology, such as a new credit or insurance product, are commonly measured using either its effect on a welfare proxy or by estimating a structural model. Using a welfare proxy is often undesirable due to noise in measurement and the challenge of converting estimated effects into a money metric, while structural approaches sometimes require strong functional form assumptions and can be skewed by unexpected moments of the data. In contrast, despite some drawbacks, Marshallian consumer surplus is frequently used as a metric for the welfare gains from access to a new product in a static setting, and with sufficient variation in prices may be relatively easy to precisely estimate. I show that under a broad class of models of dynamic optimization which nest Deaton (1991), Marshallian consumer surplus is a reasonable welfare metric for access to an intertemporal substitution technology. I demonstrate how to calculate it, and apply the approach to three experiments which randomly varied either interest rates or prices: I compare the welfare gains from grants of index insurance in Ghana to their actuarially fair value, I calculate the welfare gains to households from access to a leading MFI in Mexico, and I lower bound the foregone household welfare due to inattention to the Savers' Credit among households in the United States. In all cases, the calculation is straightforward, transparent, and can be represented graphically as a ``welfare triangle''.<br/><br/>
</details>

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


