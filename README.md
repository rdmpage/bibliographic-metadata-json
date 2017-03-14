# Representing bibliographic data in JSON

These are notes on how to model bibliographic metadata in JSON. I am assuming that if you are ready this you accept representing data as documents, and serialising those documents in JSON is a good thing to do. For example, once you have data in JSON you can store it in a database such as CouchDB, or a search index like Elasticsearch.

## BibJSON


## CSL-JSON

The Citation Style Language (CSL) is a set of standards and tools to generate formatted bibliographies in virtually any style. Although arguably the whole notion that each journal should have its own particular style for citing articles is madness, CSL has proved to be a popular tool. The files that specify the journal formats are in XML, but metadata for the article itself is often represented in JSON (see https://github.com/citation-style-language/schema#csl-json-schema). CSL-JSON has been adopted by CrossRef, and you can get CSL-JSON for any CrossRef DOI using content negotiation, or through the [CrossRef search tool](http://search.crossref.org) by clicking on the “Actions” then “Metadata as JSON” menu. Here is CSL JSON for the article [doi:10.1644/14-MAMM-A-004](http://dx.doi.org/10.1644/14-MAMM-A-004) (directly available from https://api.crossref.org/v1/works/http://dx.doi.org/10.1644/14-mamm-a-004)

```javascript
{
	"status": "ok",
	"message-type": "work",
	"message-version": "1.0.0",
	"message": {
		"indexed": {
			"date-parts": [
				[2017, 1, 9]
			],
			"date-time": "2017-01-09T20:46:47Z",
			"timestamp": 1483994807990
		},
		"reference-count": 119,
		"publisher": "Oxford University Press (OUP)",
		"issue": "5",
		"content-domain": {
			"domain": [],
			"crossmark-restriction": false
		},
		"short-container-title": ["J Mammal"],
		"cited-count": 0,
		"published-print": {
			"date-parts": [
				[2014, 10, 31]
			]
		},
		"DOI": "10.1644\/14-mamm-a-004",
		"type": "journal-article",
		"created": {
			"date-parts": [
				[2014, 10, 29]
			],
			"date-time": "2014-10-29T22:40:25Z",
			"timestamp": 1414622425000
		},
		"page": "943-959",
		"source": "CrossRef",
		"title": ["The valid generic name for red-backed voles (Muroidea: Cricetidae: Arvicolinae): restatement of the case forMyodesPallas, 1811"],
		"prefix": "http:\/\/id.crossref.org\/prefix\/10.1093",
		"volume": "95",
		"author": [{
			"given": "Michael D.",
			"family": "Carleton",
			"affiliation": []
		}, {
			"given": "Alfred L.",
			"family": "Gardner",
			"affiliation": []
		}, {
			"given": "Igor Ya.",
			"family": "Pavlinov",
			"affiliation": []
		}, {
			"given": "Guy G.",
			"family": "Musser",
			"affiliation": []
		}],
		"member": "http:\/\/id.crossref.org\/member\/286",
		"published-online": {
			"date-parts": [
				[2014, 10, 31]
			]
		},
		"container-title": ["Journal of Mammalogy"],
		"original-title": [],
		"deposited": {
			"date-parts": [
				[2017, 1, 9]
			],
			"date-time": "2017-01-09T19:46:39Z",
			"timestamp": 1483991199000
		},
		"score": 1.0,
		"subtitle": [],
		"short-title": [],
		"issued": {
			"date-parts": [
				[2014, 10, 31]
			]
		},
		"alternative-id": ["10.1644\/14-MAMM-A-004"],
		"URL": "http:\/\/dx.doi.org\/10.1644\/14-mamm-a-004",
		"ISSN": ["0022-2372", "1545-1542"],
		"issn-type": [{
			"value": "0022-2372",
			"type": "print"
		}, {
			"value": "1545-1542",
			"type": "electronic"
		}],
		"citing-count": 119,
		"subject": ["Ecology", "Animal Science and Zoology", "Genetics", "Ecology, Evolution, Behavior and Systematics", "Nature and Landscape Conservation"]
	}
}
```
The CSL-JSON is stored in the **message** key. CrossRef has added a lot of CrossRef-specific fields to do with indexing, as well as links to ORCID, FundRef, etc. Note that values for some fields can be either strings or arrays, so any software processing CSL-JSON needs to handle both data types.

An advantage of CSL-JSON is that if you want to display journal-style article citations, then the data is already in the format that the CSL tools require, making this step relatively trivial.

### Pros
- used by CrossRef (with lots of extra data)
- easy to format citations using CSL tools
- support for multiple languages in metadata

### Cons
- field names not always obvious
- format driven by supporting displaying citations


### Multilingual 

For example, the article **伊朗West Azarbaijan省切叶蜂科(膜翅目:蜜蜂总科)昆虫种类** (in English **The species of Megachilidae (Hymenoptera: Apoidea)from West Azarbaijan province, northwestern Iran**) [doi:10.15914/j.cnki.wykx.2015.31.06](http://dx.doi.org/10.15914/j.cnki.wykx.2015.31.06) can be represented like this:
```
{
	"id": "ITEM-1",
	"title": "\u4f0a\u6717West Azarbaijan\u7701\u5207\u53f6\u8702\u79d1(\u819c\u7fc5\u76ee:\u871c\u8702\u603b\u79d1)\u6606\u866b\u79cd\u7c7b",
	"abstract": "\u672c\u6587\u8bb0\u8ff0\u4e86\u4f0a\u6717\u897f\u5317\u90e8West Azarbaijan\u7701\u7684\u5207\u53f6\u8702\u79d1\u6606\u866b\u79cd\u7c7b.\u603b\u8ba1\u8bb0\u8ff08\u5c5e(Anthidium Fabricius,Chelostoma Latreille,Coelioxys Latreille,Heriades Spinola,Hoplitis Klug,Lithurgus Berthold,Megachile Latreille\u548cOsmia Panzer) 25\u79cd,\u5176\u4e2d2\u79cdMegachile(Creightonella)albisecta(KIug,1817)\u548cMegachile(Chalicodoma)parietina(Geoffroy,1785)\u4e3a\u4f0a\u6717\u5206\u5e03\u65b0\u8bb0\u5f55.",
	"multi": {
		"_key": {
			"title": {
				"zh": "\u4f0a\u6717West Azarbaijan\u7701\u5207\u53f6\u8702\u79d1(\u819c\u7fc5\u76ee:\u871c\u8702\u603b\u79d1)\u6606\u866b\u79cd\u7c7b",
				"en": "The species of Megachilidae (Hymenoptera: Apoidea)from West Azarbaijan province, northwestern Iran"
			},
			"abstract": {
				"zh": "\u672c\u6587\u8bb0\u8ff0\u4e86\u4f0a\u6717\u897f\u5317\u90e8West Azarbaijan\u7701\u7684\u5207\u53f6\u8702\u79d1\u6606\u866b\u79cd\u7c7b.\u603b\u8ba1\u8bb0\u8ff08\u5c5e(Anthidium Fabricius,Chelostoma Latreille,Coelioxys Latreille,Heriades Spinola,Hoplitis Klug,Lithurgus Berthold,Megachile Latreille\u548cOsmia Panzer) 25\u79cd,\u5176\u4e2d2\u79cdMegachile(Creightonella)albisecta(KIug,1817)\u548cMegachile(Chalicodoma)parietina(Geoffroy,1785)\u4e3a\u4f0a\u6717\u5206\u5e03\u65b0\u8bb0\u5f55."
			},
			"container-title": {
				"zh": "\u6b66\u5937\u79d1\u5b66",
				"en": "Wuyi Science Journal"
			}
		}
	},
	"type": "article-journal",
	"issued": {
		"date-parts": [
			["2015"]
		]
	},
	"author": [{
		"given": "Hassan",
		"family": "GHAHARI"
	}, {
		"given": "Neveen",
		"family": "S.GADALLAH"
	}, {
		"given": "Hassan",
		"family": "GHAHARI"
	}, {
		"given": "Neveen",
		"family": "S.GADALLAH"
	}],
	"container-title": "\u6b66\u5937\u79d1\u5b66",
	"volume": "31",
	"issue": "1",
	"page": "68-79",
	"ISSN": ["1001-4276"],
	"DOI": "10.15914\/j.cnki.wykx.2015.31.06",
	"URL": "http:\/\/d.wanfangdata.com.cn\/Periodical\/wykx201501006"
}
```

The **multi** key lists the values for different languages.


## JSON-LD

JSON-LD is linked data in JSON, see http://json-ld.org. The data structure is a set of triples, using whatever vocabulary you like. Hence it isn’t a data structure in the sense of BibJSON or CSL-JSON. Below is an example of JSON-LD “in the wild”, in this case in https://academic.oup.com/jmammal/article/95/5/943/984478/The-valid-generic-name-for-red-backed-voles [doi:10.1644/14-MAMM-A-004](http://dx.doi.org/10.1644/14-MAMM-A-004). It uses [schema.org](http://schema.org) as the vocabulary.

```javascript
{
	"@context": "http://schema.org",
	"@id": "https://academic.oup.com/jmammal/article/95/5/943/984478/The-valid-generic-name-for-red-backed-voles",
	"@type": "ScholarlyArticle",
	"name": " The valid generic name for red-backed voles (Muroidea: Cricetidae: Arvicolinae): restatement of the case for  Myodes  Pallas, 1811 ",
	"datePublished": "2014-10-31",
	"isPartOf": {
		"@id": "https://academic.oup.com/jmammal/issue/95/5",
		"@type": "PublicationIssue",
		"issueNumber": "5",
		"datePublished": "2014-10-31",
		"isPartOf": {
			"@id": "https://academic.oup.com/jmammal",
			"@type": "Periodical",
			"name": "Journal of Mammalogy",
			"issn": ["1545-1542"]
		}
	},
	"url": "http://dx.doi.org/10.1644/14-MAMM-A-004",
	"keywords": ["<italic>Clethrionomys</italic>", "<italic>Evotomys</italic>", "Lemmus", "nomenclature", "taxonomy"],
	"inLanguage": "en",
	"copyrightHolder": "American Society of Mammalogists",
	"copyrightYear": "2017",
	"publisher": "Oxford University Press",
	"sameAs": "https://academic.oup.com/jmammal/article/95/5/943/984478/The-valid-generic-name-for-red-backed-voles",
	"author": [{
		"name": "Carleton, Michael D.",
		"@type": "Person"
	}, {
		"name": "Gardner, Alfred L.",
		"@type": "Person"
	}, {
		"name": "Pavlinov, Igor Ya.",
		"@type": "Person"
	}, {
		"name": "Musser, Guy G.",
		"@type": "Person"
	}],
	"description": " In view of contradictions in the recent literature, the valid genus-group name to be applied to northern red-backed voles— Myodes Pallas, 1811, or Clethrionomys Tilesius, 1850—is reviewed. To develop the thesis that Myodes (type species, Mus rutilus Pallas, 1779) is the correct name, our discussion explores the 19th-century taxonomic works that bear on the relevant taxa, the transition in zoological codes apropos the identification of type species, and past nomenclatural habits in cases where no type species was originally indicated. We conclude that Myodes is the senior name to use for the genus-group taxon that includes the Holarctic species rutilus and frame this conclusion within a synonymy of the genus. ",
	"pageStart": "943",
	"pageEnd": "959"
}
```

### Pros
- linked data, so can be added straight to a triple store


### Cons
- not widely used
- to be useful requires that everyone agrees on the same vocabulary and why to represent identifiers

