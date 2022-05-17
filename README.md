

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

### ha több nyelvű a kérdőív

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





## kérdések 2022-05-16
- [x] a QID egyedi? igen
- [ ] az összes kitöltést le lehet így kérdezni? : 
      `select count(*) from survey_220100  where submitdate IS NOT NULL;`
 
## sql test
`select * from answers where qid=7200 and language='cs'`

multiple qid:
```
select qid, count(*) from questions GROUP BY qid  HAVING COUNT(*)>1 
## api specifications
```

```
select * from questions where qid>=905 AND qid<=914
```

```
select * from questions where qid>=6806 AND qid<=7492
```


### URL paramétrerek
pl.:
`/api/survey-followup.php?survey_id=123456&language=hu&questions[]=1&questions[]=2`

a `survey_id` kötelező, a többi opcionális

### példa válasz
```
[
  {
    "answers_total": 4188,
    "questions": [
      {
        "qid": 1,
        "label": "Nem",
        "answers": [
          {
            "code": 21,
            "label": "ferfi",
            "actual": 1758
          },
          {
            "code": 22,
            "label": "no",
            "actual": 2430
          }
        ]
      },
      {
        "qid": 2,
        "label": "Kor",
        "answers": [
          {
            "code": 21,
            "label": "12-29",
            "actual": 1064
          },
          {
            "code": 22,
            "label": "30-99",
            "actual": 3124
          }
        ]
      }
    ]
  }
]
```


## Meeting, Bea, 2022-04-26



## Axios
>Axios is a _[promise-based](https://javascript.info/promise-basics)_ HTTP Client for [`node.js`](https://nodejs.org/) and the browser. It is _[isomorphic](https://www.lullabot.com/articles/what-is-an-isomorphic-application)_ (= it can run in the browser and nodejs with the same codebase). On the server-side it uses the native node.js `http` module, while on the client (browser) it uses XMLHttpRequests.

https://axios-http.com/docs/intro

## Zoli messenger, 2022-04-19
hello; a lényeg szerintem hogy egy minta JSON filet mutass a programozónak, hogy ilyet szeretnél kapni, a többit ő majd megoldja, az a dolga

mittomén, pl az esetedben:

```
[
	{
		"survey_id": 526,
		"survey_name": "Kedvenc buszjárat",
		"number_of_responses": 2345
	},
	{
		"survey_id": 23,
		"survey_name": "Fagylalt preferenciakutatás",
		"number_of_responses": 23
	},
	{
		"survey_id": 43,
		"survey_name": "Jövőképek a bölcsödében",
		"number_of_responses": 621
	}
]
```

ennek az az előnye is van hogy a minta alapján rögtön elkezdheted fejleszteni a react frontendet

ha jól értem akkor ebben az egyszerű példádban meg kell kapnod a válaszok számát kérdőívenként

akkor a programozó csinál egy api endpointot ami elérhető valahol, pl: [https://ariosz.hu/api/v1/surveys](https://ariosz.hu/api/v1/surveys)

ha erre az URL-re küldesz egy kérést (pl. React-ból axios fetch), akkor visszakapod ezt a JSON-t

ha szűrni is kell, akkor kéred hogy adhass paramétereket

pl. [https://ariosz.hu/api/v1/surveys?customer=molzrt](https://ariosz.hu/api/v1/surveys?customer=molzrt)

vagy [https://ariosz.hu/api/v1/surveys?start_date=2022-01-01&end_date=2022-02-01](https://ariosz.hu/api/v1/surveys?start_date=2022-01-01&end_date=2022-02-01)

__bezdi: az axios fetch az egy library?__

[https://reactjs.org/docs/faq-ajax.html](https://reactjs.org/docs/faq-ajax.html)

Axios az egyik népszerűbb API hívó könyvtár, de nem muszáj azt használnod [https://github.com/axios/axios](https://github.com/axios/axios)


## 2022-04-12

szerdán hívni a Borbás Beát\


## Node

-   [Build a REST API with Node.js, Express, and MySQL](https://blog.logrocket.com/build-rest-api-node-express-mysql/)
