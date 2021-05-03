---
id: 306
title: SEO – Keyword ranking with Google Analytics
date: 2009-09-07T13:17:45+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=306
permalink: /seo-keyword-ranking-with-google-analytics/
dsq_thread_id:
  - "43376981"
categories:
  - CentOS
tags:
  - Google
  - SEO
---
<img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2009/09/google.png" alt="google" title="google" width="143" height="58" class="alignleft size-full wp-image-398" />  
Google is busy beta testing their new AJAX driven search engine and with the release of this new AJAX version it&#8217;s now possible, with the help of Filters, to allow Google Analytics to track the clicked position of specific keywords on a Google Keyword Search.

Firstly see if you&#8217;re using the new search engine, preform a search and then look at the url of the result page. This will assist you later went preforming keyword searches in Google in order to populate you Google Analytics data (most countries don&#8217;t support it as of yet).

**Standard:**  
<img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2009/09/google\_filter\_search2.jpg" alt="google\_filter\_search2" title="google\_filter\_search2" width="473" height="36" size-full wp-image-327" srcset="https://www.how2centos.com/wp-content/uploads/2009/09/google\_filter\_search2.jpg 473w, https://www.how2centos.com/wp-content/uploads/2009/09/google\_filter\_search2-300x22.jpg 300w" sizes="(max-width: 473px) 100vw, 473px" />

**AJAX driven:**  
<img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2009/09/google\_filter\_search1.jpg" alt="google\_filter\_search1" title="google\_filter\_search1" width="519" height="37" size-full wp-image-331" srcset="https://www.how2centos.com/wp-content/uploads/2009/09/google\_filter\_search1.jpg 519w, https://www.how2centos.com/wp-content/uploads/2009/09/google\_filter\_search1-300x21.jpg 300w" sizes="(max-width: 519px) 100vw, 519px" />

<!--more-->

  
**Adding the new filters**

Select the Analytics you want to start tracking and &#8220;+ Add new profile&#8221;  
<img loading="lazy" class="aligncenter size-medium wp-image-343" title="google_filter_howto1" src="http://www.how2centos.com/wp-content/uploads/2009/09/google_filter_howto1.png" alt="google_filter_howto1" width="248" height="83" srcset="https://www.how2centos.com/wp-content/uploads/2009/09/google_filter_howto1.png 591w, https://www.how2centos.com/wp-content/uploads/2009/09/google_filter_howto1-300x100.png 300w" sizes="(max-width: 248px) 100vw, 248px" /> 

Add a profile for an existing domain  
<img loading="lazy" class="aligncenter size-medium wp-image-344" title="google_filter_howto2" src="http://www.how2centos.com/wp-content/uploads/2009/09/google_filter_howto2.png" alt="google_filter_howto2" width="148" height="34" srcset="https://www.how2centos.com/wp-content/uploads/2009/09/google_filter_howto2.png 353w, https://www.how2centos.com/wp-content/uploads/2009/09/google_filter_howto2-300x69.png 300w" sizes="(max-width: 148px) 100vw, 148px" /> 

Choose a profile name, I just append &#8220;rankings&#8221; onto my existing profile name  
<img loading="lazy" class="aligncenter size-medium wp-image-345" title="google_filter_howto3" src="http://www.how2centos.com/wp-content/uploads/2009/09/google_filter_howto3.png" alt="google_filter_howto3" width="320" height="148" srcset="https://www.how2centos.com/wp-content/uploads/2009/09/google_filter_howto3.png 763w, https://www.how2centos.com/wp-content/uploads/2009/09/google_filter_howto3-300x138.png 300w" sizes="(max-width: 320px) 100vw, 320px" /> 

Once the profile is created select &#8220;Edit&#8221; to begin adding your Google filters  
<img loading="lazy" class="aligncenter size-full wp-image-346" title="google_filter_howto4" src="http://www.how2centos.com/wp-content/uploads/2009/09/google_filter_howto4.png" alt="google_filter_howto4" width="389" height="44" srcset="https://www.how2centos.com/wp-content/uploads/2009/09/google_filter_howto4.png 926w, https://www.how2centos.com/wp-content/uploads/2009/09/google_filter_howto4-300x34.png 300w" sizes="(max-width: 389px) 100vw, 389px" /> 

&#8220;+ Add Filter&#8221; to add your first Google filter  
<img loading="lazy" class="aligncenter size-medium wp-image-347" title="google_filter_howto5" src="http://www.how2centos.com/wp-content/uploads/2009/09/google_filter_howto5.png" alt="google_filter_howto5" width="164" height="40" srcset="https://www.how2centos.com/wp-content/uploads/2009/09/google_filter_howto5.png 392w, https://www.how2centos.com/wp-content/uploads/2009/09/google_filter_howto5-300x72.png 300w" sizes="(max-width: 164px) 100vw, 164px" /> 

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"><pre>
Filter name: "Ranking 1"
Filter type: "Custom filter - Include"
Filter field: "Campaign Medium"
Filter pattern: "organic"
</pre>


<p>
  <img loading="lazy" class="aligncenter size-full wp-image-314" title="google_filter_ranking1" src="http://www.how2centos.com/wp-content/uploads/2009/09/google_filter_ranking1.jpg" alt="google_filter_ranking1" width="461" height="443" srcset="https://www.how2centos.com/wp-content/uploads/2009/09/google_filter_ranking1.jpg 461w, https://www.how2centos.com/wp-content/uploads/2009/09/google_filter_ranking1-300x288.jpg 300w" sizes="(max-width: 461px) 100vw, 461px" />
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">


<pre>
Filter name: "Ranking 2"
Filter type: "Custom filter - Include"
Filter field: "Campaign Source"
Filter pattern: "google"
</pre>


<p>
  <img loading="lazy" class="aligncenter size-full wp-image-315" title="google_filter_ranking2" src="http://www.how2centos.com/wp-content/uploads/2009/09/google_filter_ranking2.jpg" alt="google_filter_ranking2" width="457" height="439" srcset="https://www.how2centos.com/wp-content/uploads/2009/09/google_filter_ranking2.jpg 457w, https://www.how2centos.com/wp-content/uploads/2009/09/google_filter_ranking2-300x288.jpg 300w" sizes="(max-width: 457px) 100vw, 457px" />
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">


<pre>
Filter name: "Ranking 3"
Filter type: "Custom filter - Advanced"
Field A -&gt; Extract A: "Campaign term", "(.*)"
Field B -&gt; Extract B: "Referral", "cd=([0-9]+)"
Output To -&gt; User Defined: "$A1 (position: $B1)"
</pre>


<p>
  <img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2009/09/google_filter_ranking3.png" alt="google_filter_ranking3" title="google_filter_ranking3" width="550" height="450" class="aligncenter size-full wp-image-389" />
</p>


<p>
  Finally a filter to add an &#8220;unknown position&#8221; message when the position of the searched keyword is not passed through:
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">


<pre>
Filter name: "Ranking 4"
Filter type: "Custom filter - Search and Replace"
Filter field: "User Defined"
Search String: "(.*)\(position: \)"
Replace String: "(position unknown)"
</pre>


<p>
  <img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2009/09/google_filter_ranking4-298x300.png" alt="google_filter_ranking4" title="google_filter_ranking4" width="298" height="300" class="aligncenter size-medium wp-image-372" srcset="https://www.how2centos.com/wp-content/uploads/2009/09/google_filter_ranking4-298x300.png 298w, https://www.how2centos.com/wp-content/uploads/2009/09/google_filter_ranking4-150x150.png 150w, https://www.how2centos.com/wp-content/uploads/2009/09/google_filter_ranking4.png 453w" sizes="(max-width: 298px) 100vw, 298px" />
</p>


<p>
  You can view your newly created ranking results under &#8220;visitors &#8211;> user defined&#8221;
</p>


<p>
  <img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2009/09/google_filter_howto6-255x300.png" alt="google_filter_howto6" title="google_filter_howto6" width="255" height="300" class="aligncenter size-medium wp-image-369" srcset="https://www.how2centos.com/wp-content/uploads/2009/09/google_filter_howto6-255x300.png 255w, https://www.how2centos.com/wp-content/uploads/2009/09/google_filter_howto6.png 502w" sizes="(max-width: 255px) 100vw, 255px" />
</p>


<p>
  After a couple of hours you should start seeing the results, bearing in mind that the new search referring URL is still in beta and not available in some regions.
</p>


<p>
  The results will look something like this:
</p>


<p>
  <img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2009/09/google_filter_howto7.png" alt="google_filter_howto7" title="google_filter_howto7" width="542" height="109" class="aligncenter size-full wp-image-393" srcset="https://www.how2centos.com/wp-content/uploads/2009/09/google_filter_howto7.png 542w, https://www.how2centos.com/wp-content/uploads/2009/09/google_filter_howto7-300x60.png 300w" sizes="(max-width: 542px) 100vw, 542px" />
</p>


<p>
  <strong>Conclusion:</strong>
</p>


<p>
  The most challenging part of working with Google Analytics is waiting for your changes to take effect. Once a filter of profile setting is changed, it may be seven or more hours before the data in Google Analytics is affected. Debugging problems in Google Analytics takes time, so be patient. The more you can do to understand the implications of a change, prior to making the modification, the better.
</p>


<p>
  References:<br />
  <a href="http://yoast.com/track-seo-rankings-and-sitelinks-with-google-analytics-ii/#utm_source=rss&#038;utm_medium=rss&#038;utm_campaign=track-seo-rankings-and-sitelinks-with-google-analytics-ii">Yoast.com</a><br />
  <a href="http://www.analyticsexperts.com/resources/google-analytics-regex-filter-tester/">Regular Expression Filter Tester (BETA)*</a>
</p>