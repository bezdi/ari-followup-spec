## Paraméterek

|paraméter|kötelező|leírás|
|---------|---------|---------|
| survey_id | igen |A survey ID-ja|
|~~language~~|~~nem~~|~~szűrni lehet adott nyelvre~~|
| last page | nem | A kérdőív utolsó oldalának a száma. Ebből tudjuk, hogy végigért-e a kérdőíven a kitöltő |
| quality|nem|A kitöltés min. ideje mp-ben. _'Lastpage' paraméter nélkül nincs értelme_. A kitöltés ideje a startdate és a datestamp oszlopok különbségéből számolható||
| questions|nem|Tömb, ha nincs megadva az összes kérdést visszakapjuk|

## Paraméter példa

```
?survey_id=210099
&lastpage=46
&quality=600
&questions[]=6814
&questions[]=6879
&questions[]=6880
```


## JSON válasz példa
> __note:__ 
`select COUNT(DISTINCT startlanguage) as number_of_languages FROM survey_221401` => meg lehet állapítani, hogy hány féle nyelven megy a kérdőív

### Ha egy nyelvű a kérdőív

```json
[
	{
		"answers_total":3501,
		"answers_total_finished":2641,
		"answers_total_quality": 2180,
		"questions":[
			{
				"qid": 6814,
				"title": "Q01",
				"answers": [
					{
						"code": 1,
						"label": "Male"
						"total": 1333,
						"total_finished": 1333,
						"total_quality": 1087
					},
					{
						"code": 2,
						"label": "Female"
						"total": 1308,
						"total_finished": 1308,
						"total_quality": 1093
					}
				]
			},
			{
				"qid": 6879,
				"title": "KOR5",
				"answers": [
					// ...
				]
			},	
			// ...
		]
	}
]
```

### Ha több nyelvű a kérdőív

```json
[
	{
		"answers_total": {
			"en": 3501,
			"cz": 3501,
			"hu": 3501,
			"pl": 3501,
			"sk": 3501
		},
		"answers_total_finished": {
			"en": 2641,
			"cz": 2641,
			"hu": 2641,
			"pl": 2641,
			"sk": 2641
		},
		"answers_total_quality":  {
			"en": 2180,
			"cz": 2180,
			"hu": 2180,
			"pl": 2180,
			"sk": 2180
		},
		"questions":[
			{
				"qid": 6814,
				"language": "en",
				"title": "Q01",
				"answers": [
					{
						"code": 1,
						"label": "Male"
						"total": 1333,
						"total_finished": 1333,
						"total_quality": 1087
					},
					{
						"code": 2,
						"label": "Female"
						"total": 1308,
						"total_finished": 1308,
						"total_quality": 1093
					}
				]
			},
			{
				"qid": 6814,
				"language": "hu",
				"title": "Q01",
				"answers": [
					// ...
				]
			},	
			// ...
		]
	}
]
```


