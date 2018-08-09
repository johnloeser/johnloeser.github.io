---
layout: nosidebar
title: Research
permalink: /research/
---

## Work in progress

<hr/>
<details title="ite">
<summary><strong>JMP</strong> <u>Inframarginal treatment effects: Estimating the welfare losses from groundwater depletion in India</u><br/>
(2018 Aug)<hr/></summary>
<br/>Market failures in the management of groundwater, a common pool resource, have contributed to groundwater depletion in arid districts in India. An estimate of the effect of groundwater depletion on welfare is necessary to determine the optimal policy response. This paper does so by defining and estimating the Inframarginal Treatment Effect (ITE), the effect on welfare implied by an observed change in adoption of treatment (in this paper, irrigation) caused by a shift in potential utility under treatment (in this paper, groundwater depletion). I derive sufficient conditions for the ITE to be well defined and policy invariant. I show that a Local Average ITE can be estimated from the difference between two linear IV estimators, the first of which uses an instrument satisfying a non-standard exclusion restriction. This approach extends conventional revealed preference approaches by allowing estimation of welfare effects without an observable price of treatment. In my setting, I construct instruments for costs of irrigation using hydrogeological characteristics, and for potential revenue under irrigation using climate and soil characteristics. The estimated Local Average ITE implies a one standard deviation decrease in access to groundwater would cause welfare losses equal to 12.4% of agricultural revenue, while the estimated LATE implies observed agricultural revenue would fall by just 8.9%.
</details>
<hr style="visibility:hidden;height:12pt" />
<hr/>
<details title="cs">
<summary><u>Marshallian consumer surplus from intertemporal substitution: Applications to savings, credit, and index insurance</u><br/>
(2017 Dec) <!--include link to paper here, should be href="{{ site.baseurl }}/assets/" with link name "paper"--><hr/></summary>
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


