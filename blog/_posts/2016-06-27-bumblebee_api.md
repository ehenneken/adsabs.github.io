---
layout: blog_post
title: A Tourist Map of the Bumblebee API Landscape
author:  Edwin Henneken
position: ADS IT Specialist
category: blog
label: technical
thumbnail: blog/images/API_title_image.png
comment:
---

Tourist maps usually highlight locations of interest, without drowning you in all kinds of unnecessary details. Not that those details are not interesting, but they are for a different type of visit. In this blog you will learn about some interesting places and how to make the most out of your currency (i.e. "rate limit") in ADS API Land. There are multiple ways to enter the API landscape and there is no way to cover them all. In this blog you will find some specific examples (which you are free to generalize or port to different environments) and some general pointers on efficient queries. Before looking at some specific examples, I will give some tips on how to use the API efficiently.

Since so many people use Andy Casey's client for the ADS API, I will illustrate the princples using this client. In general, even before writing any code, it is a good investment of your time to think about what you want to retrieve from the API. For example: do you want to retrieve data based on a subject, or do you already have identifiers for publications (like DOIs, arXiv identifiers or bibcodes) and do you want to retrieve data based on these identifiers. You should also think about what information you want to retrieve (like author information, publication titles and abstracts, citations). Often it is possible to retrieve a lot of information in a single query. If you think you know what you are looking for and you are using Andy Casey's client, there is one step you may want to consider, before contacting the actual API: use the "sandbox" environment to prototype your scenario. This environment will allow you to test your code against mock data, leaving your precious rate limit in tact! Of course, there is nobody stopping you from bombarding the API with queries immediately, but if you exceed your rate limit, you will have to wait until it will get refreshed. It is 5000 queries per 24 hours! The rate resetting happens at midnight UTC. The sandbox enviroment is really meant to see whether you are getting the information you want; it will not tell you how quickly you are burning up your precious rate limit. For that you need to be aware of a number of general principles, which I will come to in a moment. First a sketch of how to use the sandbox environment. Once you have installed the client, all you need to do is write a script that imports the client like

    import ads.sandbox as ads

which will give you access to all of the functionality of the regular API client, but when it executes queries, instead of contacting the ADS API, it runs these queries against stub data, included with the client. 

In general, when you query the ADS API, it returns a set of documents in its reponse and each of these documents has attributes (see: https://github.com/adsabs/adsabs-dev-api/blob/master/search.md#fields). Here we meet one of the first important general principles: not all of data associated with these attributes is sent back in the response. This is the so-called principle of "lazy loading". By default, data for the following fields is sent back in the response: "author", "first_author", "bibcode", "id", "year", "title", "reference", "citation". If you request the data for any other field, it will need to be retrieved, resulting in additional API queries, costing you precious API currency! For example, if you search for all publications where "NASA" appears somewhere in an author affiliation and you want titles and abstracts, you would do (for example)

    import ads
	ads.config.token = '<your API token>' # I'll leave out these two lines in the other examples
	q = ads.SearchQuery(aff="*NASA*")
	for paper in q:
	    print paper.title, paper.abstract
	    ....
		
This looks like 1 API request, but because "abstract" is not a default field, the above will result in 1 API query *plus* 1 query for every document retrieved! The solution is simple, luckily: just specify the fields you need explicitly and there will be no surprises. So, when you do

	q = ads.SearchQuery(aff="*NASA*", fl=['id','bibcode','title','abstract'])
	for paper in q:
	    print paper.title, paper.abstract
	    ....
		
you really will be doing just 1 API request. Another tidbit of useful information is the amount of records returned by default: the SearchQuery class used in the examples, so far, returns 50 records at one time, and up to 3 pagination requests (the default value of the "max_pages" parameter, to prevent deep pagination of results). So, the default settings will get you 200 records. You can increase the "max_pages" value by including it in your request, like e.g.

    q = ads.SearchQuery(aff="*NASA*", fl=['id','bibcode','title','abstract'], max_pages=100)

but be aware that each new page fetched will count against your daily API limit! So, for exploration-type queries (i.e. you search for information using keywords and search terms), the best way to be economic with your API limit is to determine in advance what fields you are interested in (maybe through experimenting with the sandbox environment).

A different type of information retrieval is when you already know publication identifiers (like bibcodes, DOIs or arXiv identifiers). A typical use case is: you have a bibliography and you want to keep track of citations and read counts. Just like in the case of exploration-type queries, you need to determine in advance which fields you are interested in, so that you can request these explicitly. Let's say that for a list of papers (for which you know their identifiers) you want to retrieve authors, title, abstract, citation count and reads count. You could simply do

	papers = ['10.1111/j.1365-2966.2009.15627.x', 'arXiv:1406.2762', '2015MNRAS.447.1282N']
	for paper in papers:
	    q = ads.SearchQuery(q="identifier:%s"%paper, fl=['bibcode','author','title','abstract','citation_count','read_count'])
		... [do something with the information retrieved]
		
and see your precious rate limit be decreased by the number of papers. A much more efficient way would be

	papers = ['10.1111/j.1365-2966.2009.15627.x', 'arXiv:1406.2762', '2015MNRAS.447.1282N']
	q = ads.SearchQuery(q="identifier:(%s)" % " OR ".join(papers), fl=['bibcode','author','title','abstract','citation_count','read_count'])
	
and get the same information with just 1 query. Keep in mind that a query string should be at most 1000 characters. The "identifier" field is an array of alternative identifiers for the record. It may contain alternative bibcodes, DOIs and/or arXiv identifiers. 

Finally, I will highlight some queries that belong more on a USGS map than a Tourist Map. Some great examples using Andy Casey's module are listed here: https://github.com/adsabs/ads-examples. In what follows I will illustrate some more complex API queries you can do by communicating with the API end point directly. Here is a fun API query for you to parse

    curl -H "Authorization: Bearer <YOU API KEY HERE>" 'https://api.adsabs.harvard.edu/v1/search/query?facet=true&
	facet.minCount=1&
	facet.pivot=year%2Ccitation_count&
	q=bibgroup:CfA&
	sort=citation_count%20desc&
	stats=true&
	stats.field=citation_count'

This is an example of a "pivot query" with an added bonus of some statistics calculated over the query. Pivoting is a summarization tool that lets you automatically sort, count, total or average data stored in a table. The results are typically displayed in a second table showing the summarized data. Pivot faceting lets you create a summary table of the results from a faceting documents by multiple fields. Sometimes you want to have some basic metrics over a large set of papers and the (current) maximum of 2000 papers for the Citation Metrics utility is just too restrictive. What the above query does is execute a query (in this case: "get all publications from the CfA bibliography") and then requests histograms over citation counts with values sorted by citation count (descending), split up per publication year. In addition this query requests some basic statistics over the data retrieved (citation counts). In the JSON structure returned you will see entries like

    [{"field":"year","value":"2006","count":2650,"pivot":[<pivot data>]}, ...]

with pivot data looking like

    {"field":"citation_count","value":0,"count":1650},{"field":"citation_count","value":1,"count":129},...

The "stats" part adds data of the following form

    "stats":{"stats_fields":{"citation_count":{"min":0.0,"max":9448.0,"count":50266,"missing":0,
	"sum":1111857.0,"sumOfSquares":3.84760925E8,"mean":22.119464449130625,"stddev":84.6484992170285,"facets":{}}}}

Another fun query is

    https://api.adsabs.harvard.edu/v1/search/query?facet=true&
	q=bibgroup:CfA&
	facet.limit=10&
	facet.mincount=1&
	facet.field=bibstem&
	fq=citation_count%3A%5B100%20TO%20*%5D

which gives you the top 10 journals contributing to publications in the CfA bibliography with 100 or more citations (and tells you exactly how many papers from these journals, for this bibgroup, have at least 100 citations).

One last example! You want to use the API to look for a term, but want to check the context in which it was mentioned. 

    https://api.adsabs.harvard.edu/v1/search/query?q=MMT&
	fq=bibgroup:CfA&
	hl=true&
	indent=true&
	hl.fl=title,abstract,body,ack&
	fl=bibcode,author,title,id&
	rows=500

For an unfielded query for the term "MMT" within the CfA bibliography, this query applies highlighting (i.e. it puts an emphasis HTML tag around the match) and shows snippets if the match was found in the title, abstract, full text and/or acknowledgement of a publication.