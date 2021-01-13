---
layout: nosidebar
title: research
permalink: /research/
---

## Working papers
<hr/>
<details title="bkkll">
<summary><a title="bkkll.pdf" href="{{ site.baseurl }}/assets/bkkll.pdf">Sectoral heterogeneity in the COVID-19 recovery: Evidence from Rwanda</a> (w/ Kieran Byrne, Saahil Karpe, <a title="Florence Kondylis" href="https://sites.google.com/site/decrgkondylis/">Florence Kondylis</a>, and <a title="Megan Lang" href="https://www.meganlangecon.com/">Megan Lang</a>)<br/>
(2021 Jan) <ar>show abstract</ar>&nbsp;<a title="bkkll.pdf" href="{{ site.baseurl }}/assets/bkkll.pdf">working paper</a><hr/></summary>
<br/>Following the initial COVID-19 shock, developing countries have begun to transition to a COVID-19 economic recovery characterized by eased lockdowns and fiscal stimulus. We leverage high frequency administrative tax records from Rwanda on firm sales and employment to characterize the impacts of the COVID-19 shock and recovery. We show that the aggregate shock peaked in April 2020, with aggregate turnover and employment recovering to pre-COVID-19 levels by September. The aggregate recovery masks meaningful heterogeneity: while the initial shock impacted sectors in which in-person work was most necessary, the sectors in which face-to-face interactions with consumers are most necessary continue to experience a protracted recovery.
</details>
<hr style="visibility:hidden;height:12pt" />
<hr/>
<details title="jklm">
<!--<summary><a title="jklm.pdf" href="{{ site.baseurl }}/assets/jklm.pdf">Factor Market Failures and the Adoption of Irrigation in Rwanda</a> (w/ <a title="Maria Jones" href="https://www.worldbank.org/en/about/people/m/maria-ruth-jones">Maria Jones</a>, <a title="Florence Kondylis" href="https://sites.google.com/site/decrgkondylis/">Florence Kondylis</a>, and <a title="Jeremy Magruder" href="https://are.berkeley.edu/~jmagruder/">Jeremy Magruder</a>)<br/>
(2019 Dec) <ar>show abstract</ar>&nbsp;<a title="jklm.pdf" href="{{ site.baseurl }}/assets/jklm.pdf">pdf</a><hr/></summary>-->
<summary><a title="working paper (WPS)" href="http://documents.worldbank.org/curated/en/496531576687282363/Factor-Market-Failures-and-the-Adoption-of-Irrigation-in-Rwanda">Factor market failures and the adoption of irrigation in Rwanda</a> (w/ <a title="Maria Jones" href="https://www.worldbank.org/en/about/people/m/maria-ruth-jones">Maria Jones</a>, <a title="Florence Kondylis" href="https://sites.google.com/site/decrgkondylis/">Florence Kondylis</a>, and <a title="Jeremy Magruder" href="https://are.berkeley.edu/~jmagruder/">Jeremy Magruder</a>)<br/>
(2020 Mar) <ar>show abstract</ar>&nbsp;<a title="working paper (WPS)" href="http://documents.worldbank.org/curated/en/496531576687282363/Factor-Market-Failures-and-the-Adoption-of-Irrigation-in-Rwanda">working paper (WPS)</a>&nbsp;<a title="working paper (NBER)" href="https://www.nber.org/papers/w26698">working paper (NBER)</a><hr/></summary>
<br/>We examine constraints to adoption of new technologies in the context of hillside irrigation schemes in Rwanda. We leverage a plot-level spatial regression discontinuity design to produce 3 key results. First, irrigation enables dry season horticultural production, which boosts on farm cash profits by 44-71%. Second, adoption is constrained: access to irrigation causes farmers to substitute labor and inputs away from their other plots. Eliminating this substitution would increase adoption by at least 21%. Third, this substitution is largest for smaller households and wealthier households. This result can be explained by labor market failures in a standard agricultural household model.
</details>
<hr style="visibility:hidden;height:12pt" />
<hr/>
<details title="mse">
<summary><strong>JMP</strong> <a title="jmp.pdf" href="{{ site.baseurl }}/assets/jmp.pdf">The treatment effect elasticity of demand: Estimating the welfare losses from groundwater depletion in India</a><br/>
(2018 Nov) <ar>show abstract</ar>&nbsp;<a title="jmp.pdf" href="{{ site.baseurl }}/assets/jmp.pdf">pdf</a><hr/></summary>
<br/>I estimate an elasticity of irrigation adoption to its gross returns in rural India. Many approaches to estimating this elasticity fail when agents select into adopting irrigation on heterogeneous gross returns and costs. I develop a novel approach to correct for selection using two instrumental variable estimators that can be implemented with aggregate data on gross revenue and adoption of irrigation. I use climate and soil characteristics as an instrument for gross returns to irrigation, and hydrogeology as an instrument for irrigation to correct for selection. I estimate that a 1% increase in the gross returns to irrigation causes a 0.7% increase in adoption of irrigation. I use this elasticity to infer changes in profits from changes in adoption of irrigation caused by shocks to its profitability, and to conduct counterfactuals. First, groundwater depletion from 2000-2010 in northwestern India permanently reduced economic surplus by 1.2% of gross agricultural revenue. Second, I evaluate a policy that optimally reduces relative subsidies for groundwater irrigation in districts with large negative pumping externalities, while holding total subsidies fixed. Under the policy, depletion caused by subsidies decreases by 16%, but farmer surplus increases by only 0.07% of gross agricultural revenue.
</details>
<hr style="visibility:hidden;height:12pt" />

## Work in progress
<hr/>
<details title="cs">
<summary>Marshallian consumer surplus from intertemporal substitution: Applications to savings, credit, and index insurance<br/>
(2017 Dec) <ar>show abstract</ar><!--include link to paper here, should be href="{{ site.baseurl }}/assets/" with link name "paper"--><hr/></summary>
<br/>The welfare gains from a novel intertemporal substitution technology, such as a new credit or insurance product, are commonly measured using either its effect on a welfare proxy or by estimating a structural model. Using a welfare proxy is often undesirable due to noise in measurement and the challenge of converting estimated effects into a money metric, while structural approaches sometimes require strong functional form assumptions and can be skewed by unexpected moments of the data. In contrast, despite some drawbacks, Marshallian consumer surplus is frequently used as a metric for the welfare gains from access to a new product in a static setting, and with sufficient variation in prices may be relatively easy to precisely estimate. I show that under a broad class of models of dynamic optimization which nest Deaton (1991), Marshallian consumer surplus is a reasonable welfare metric for access to an intertemporal substitution technology. I demonstrate how to calculate it, and apply the approach to three experiments which randomly varied either interest rates or prices: I compare the welfare gains from grants of index insurance in Ghana to their actuarially fair value, I calculate the welfare gains to households from access to a leading MFI in Mexico, and I lower bound the foregone household welfare due to inattention to the Savers' Credit among households in the United States. In all cases, the calculation is straightforward, transparent, and can be represented graphically as a ``welfare triangle''.
</details><br/>

<!--## Work in progress-->

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


