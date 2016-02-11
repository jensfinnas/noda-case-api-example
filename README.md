## JSON API Documentation

#### Response:

```
// This:
jQuery.getJSON('http://nodabase.net/cases/?json',{},function(data, status){
    console.log(data);    
}

// Will return something like this:
{
    "max_num_pages": 2, // Total amount of pages
    "post_count": 10,   // Amount of posts in the current query.
    "found_posts": 16,  // Total amount of posts found.
    "paged": 1,         // Current page.
    "posts": [...]      // Array of post objects
}
```


#### Queries:
The base query system is almost a direct implementation of the WordPress WP_Query, only with limitation regarding security.

All meta_query and tax_query features are implemented and can be used according to the [WP_Query](https://codex.wordpress.org/Class_Reference/WP_Query) documentation.

##### Accepted parameters.
* posts_per_page
* paged
* meta_query
* tax_query
* post__not_in


This request:
```
jQuery.getJSON('http://nodabase.dev/cases/?json', {
    posts_per_page: 10,
    paged:1,
    meta_query: { 
        relation: 'AND',
        0: {
            key: 'case_competition',
            value: 'not-in-competition',
            compare: '='
        },
        1: {
            key: 'case_status',
            value: 'submission',
            compare: '='
        }
    },
    tax_query: {
        relation: 'AND',
        0: {
            taxonomy: 'publisher-category',
            field: 'slug',
            terms: 'norway'
        }
    }
}, function (data, status) {
    console.log(data);
});
```

Equals this: `http://nodabase.dev/cases/?json&posts_per_page=10&meta_query[0][key]=case_competition&meta_query[0][value]=not-in-competition&meta_query[0][compare]==&meta_query[1][key]=case_status&meta_query[1][value]=submission&meta_query[1][compare]==&meta_query[relation]=AND&tax_query[0][taxonomy]=publisher-category&tax_query[0][field]=slug&tax_query[0][terms]=norway&tax_query[relation]=AND`

###### include_embeds
```
// This:
jQuery.getJSON('http://nodabase.net/cases/?json',{
    include_embeds:'true',  //Forces the request to include countries, tags, and topic_tags. 
},function(data, status){
    console.log(data);    
}

// Will return something like this:
{
    "max_num_pages": 2,     // Total amount of pages
    "post_count": 10,       // Amount of posts in the current query.
    "found_posts": 16,      // Total amount of posts found.
    "paged": 1,             // Current page.
    "countries": [...],     // Array of category objects
    "topic_tags": [...],    // Array of category objects
    "tags": [...],          // Array of category objects
    "posts": [...]          // Array of post objects
}
```

#### Post object:

```
{
    "id": 523,
    "title": {
        "raw": "#kandulova",
        "rendered": "#kandulova"
    },
    "content": {
        "raw": "<h2>Description<\/h2>\r\n#kandulova is Mittmedias way of vreating an arena for our audience where they could get the straight answers they always wanted from politicians. It launched about a month before the Swedish elections in 2014 and we collected about 2000 questions from our readers and we also got about 12 000 answers from politicians.\r\n<h2>Data<\/h2>\r\nAll questions crowdsourced through web forms our 11 news sites. Front-end hosted on news site communicating with a Restful API on the backend side. The application was mostly built using Ruby on Rails and all the data (questions and answers) were stored in a PostgreSQL database. All communication was events-based which facilitated automated e-mails to the 353 politicians and the people who were asking questions. We also built a custom-made light CMS to handle all the politicians, political parties, etc.\r\n<h2>Process<\/h2>\r\n<p class=\"MsoNormal\"><span lang=\"EN-US\">MittMedia is a media conglomerate covering one third of Sweden\u00e2\u0080\u0099s surface. We put out 19 morning newspapers and run 11 websites, all focused on local journalism. In the fall of 2013 we all together started to discuss how we would cover the upcoming Swedish election of 2014 with focus on the local elections.<\/span><\/p>\r\n<p class=\"MsoNormal\"><span lang=\"EN-US\">We quickly identified that we wanted to give our audience an arena where they themselves could set the agenda for the political discussion in their municipality. Far too often politicians avoid the straight questions. Will there be a new road? Will you save our local school? We wanted to make a difference for the democracy in our local societies and give every person a direct line to the politicians, putting them on the spot. Before we started designing the final solution we talked to our local community to see if they liked the idea and then we did user tests with politicians to make sure the technical solution would fit their busy schedules and that they supported the concept.<\/span><\/p>\r\n<p class=\"MsoNormal\"><b><span lang=\"EN-US\">The application<\/span><\/b><\/p>\r\n<p class=\"MsoNormal\"><span lang=\"EN-US\">The final web application <b>#kandulova<\/b> (translated \u00e2\u0080\u009ccan you promise?\u00e2\u0080\u009d) allowed people to ask all parties in their municipality direct questions and then get the respective answers straight from the politicians as well as visualize all the questions and the politicians answers.<\/span><\/p>\r\n<p class=\"MsoNormal\"><span lang=\"EN-US\">The reader simply asked a question through a web form on one of our responsive news sites (log in was mandatory, signing with a name wasn\u00e2\u0080\u0099t) and they also filled in which municipality the question belonged to. Every party in that municipality then had a representative whom received an alert via e-mail telling them there was a new question to answer. When the question had been answered the reader got notified by e-mail and could easily see the reply, comment it or share it in social media. <\/span><\/p>\r\n<p class=\"MsoNormal\"><span lang=\"EN-US\">We spent a lot of time recruiting and teaching politicians from 52 municipalities how to use the application. This was mainly \u00e2\u0080\u009cmanual\u00e2\u0080\u009d work where we contacted politicians, presented the idea and then offered them to participate. We got a lot of response from them afterwards that they felt like this was an important way for them to communicate with the voters and that they liked the model for it. In fact in one municipality a politician suggested that they themselves should create a similar service for the voters that should always be open, citing #kandulova as an inspiration.<\/span><\/p>\r\n<p class=\"MsoNormal\">Resources<\/p>\r\n<p class=\"MsoNoSpacing\"><span lang=\"EN-US\">Since it involved all our titles and spanned a third of Sweden\u00e2\u0080\u0099s surface it involved a lot of people. Editors, researchers, UX, design, two developers, project leaders plus Jens Finn\u00c3\u00a4s and Peter Grensund from Journalism++ Stockholm who helped a lot with developing the initial concept and how to retrieve the data for afterwards journalism.<\/span><\/p>\r\n\r\n<h2>Results<\/h2>\r\n<p class=\"MsoNormal\"><span lang=\"EN-US\">We collected all the answers on the news sites, divided by municipality. The readers could easily see which questions had been answered by which parties as well as interact by commenting or sharing through social media. The site also became a guide to the readers were they could read up on the opinions of the different parties in different matters.<\/span><\/p>\r\n<p class=\"MsoNormal\"><span lang=\"EN-US\">From this we then were able to get hundreds of ideas on how we could produce relevant journalism in different forms. It resulted in hundreds of articles, online tv and 38 live broadcasted debates.<\/span><\/p>\r\n<p class=\"MsoNormal\"><span lang=\"EN-US\">The application rolled out on our sites in August, about a month before the election. And the response was immediate. More than 2\u00c2\u00a0000 questions and 12\u00c2\u00a0000 answers came in before the elections. In total there were 353 logged in politicians spanning 19 political parties and 52 municipalities.<\/span><\/p>\r\n<p class=\"MsoNormal\"><span lang=\"EN-US\">And the best part of it might be what we now will be able to do throughout this term of office. We now have a database with all the promises the parties made and we can now easily do follow ups and nail down important questions and demand accountability. #kandulova was also nominated for \u00e2\u0080\u009cStora Journalistpriset\u00e2\u0080\u009d which is the Swedish equivalent to the Pulitzer Prize in the category \u00e2\u0080\u009cInnovator of the year\u00e2\u0080\u009d.<\/span><\/p>",
        "rendered": "<h2>Description<\/h2>\n<p>#kandulova is Mittmedias way of vreating an arena for our audience where they could get the straight answers they always wanted from politicians. It launched about a month before the Swedish elections in 2014 and we collected about 2000 questions from our readers and we also got about 12 000 answers from politicians.<\/p>\n<h2>Data<\/h2>\n<p>All questions crowdsourced through web forms our 11 news sites. Front-end hosted on news site communicating with a Restful API on the backend side. The application was mostly built using Ruby on Rails and all the data (questions and answers) were stored in a PostgreSQL database. All communication was events-based which facilitated automated e-mails to the 353 politicians and the people who were asking questions. We also built a custom-made light CMS to handle all the politicians, political parties, etc.<\/p>\n<h2>Process<\/h2>\n<p class=\"MsoNormal\"><span lang=\"EN-US\">MittMedia is a media conglomerate covering one third of Sweden\u00e2\u0080\u0099s surface. We put out 19 morning newspapers and run 11 websites, all focused on local journalism. In the fall of 2013 we all together started to discuss how we would cover the upcoming Swedish election of 2014 with focus on the local elections.<\/span><\/p>\n<p class=\"MsoNormal\"><span lang=\"EN-US\">We quickly identified that we wanted to give our audience an arena where they themselves could set the agenda for the political discussion in their municipality. Far too often politicians avoid the straight questions. Will there be a new road? Will you save our local school? We wanted to make a difference for the democracy in our local societies and give every person a direct line to the politicians, putting them on the spot. Before we started designing the final solution we talked to our local community to see if they liked the idea and then we did user tests with politicians to make sure the technical solution would fit their busy schedules and that they supported the concept.<\/span><\/p>\n<p class=\"MsoNormal\"><b><span lang=\"EN-US\">The application<\/span><\/b><\/p>\n<p class=\"MsoNormal\"><span lang=\"EN-US\">The final web application <b>#kandulova<\/b> (translated \u00e2\u0080\u009ccan you promise?\u00e2\u0080\u009d) allowed people to ask all parties in their municipality direct questions and then get the respective answers straight from the politicians as well as visualize all the questions and the politicians answers.<\/span><\/p>\n<p class=\"MsoNormal\"><span lang=\"EN-US\">The reader simply asked a question through a web form on one of our responsive news sites (log in was mandatory, signing with a name wasn\u00e2\u0080\u0099t) and they also filled in which municipality the question belonged to. Every party in that municipality then had a representative whom received an alert via e-mail telling them there was a new question to answer. When the question had been answered the reader got notified by e-mail and could easily see the reply, comment it or share it in social media. <\/span><\/p>\n<p class=\"MsoNormal\"><span lang=\"EN-US\">We spent a lot of time recruiting and teaching politicians from 52 municipalities how to use the application. This was mainly \u00e2\u0080\u009cmanual\u00e2\u0080\u009d work where we contacted politicians, presented the idea and then offered them to participate. We got a lot of response from them afterwards that they felt like this was an important way for them to communicate with the voters and that they liked the model for it. In fact in one municipality a politician suggested that they themselves should create a similar service for the voters that should always be open, citing #kandulova as an inspiration.<\/span><\/p>\n<p class=\"MsoNormal\">Resources<\/p>\n<p class=\"MsoNoSpacing\"><span lang=\"EN-US\">Since it involved all our titles and spanned a third of Sweden\u00e2\u0080\u0099s surface it involved a lot of people. Editors, researchers, UX, design, two developers, project leaders plus Jens Finn\u00c3\u00a4s and Peter Grensund from Journalism++ Stockholm who helped a lot with developing the initial concept and how to retrieve the data for afterwards journalism.<\/span><\/p>\n<h2>Results<\/h2>\n<p class=\"MsoNormal\"><span lang=\"EN-US\">We collected all the answers on the news sites, divided by municipality. The readers could easily see which questions had been answered by which parties as well as interact by commenting or sharing through social media. The site also became a guide to the readers were they could read up on the opinions of the different parties in different matters.<\/span><\/p>\n<p class=\"MsoNormal\"><span lang=\"EN-US\">From this we then were able to get hundreds of ideas on how we could produce relevant journalism in different forms. It resulted in hundreds of articles, online tv and 38 live broadcasted debates.<\/span><\/p>\n<p class=\"MsoNormal\"><span lang=\"EN-US\">The application rolled out on our sites in August, about a month before the election. And the response was immediate. More than 2\u00c2\u00a0000 questions and 12\u00c2\u00a0000 answers came in before the elections. In total there were 353 logged in politicians spanning 19 political parties and 52 municipalities.<\/span><\/p>\n<p class=\"MsoNormal\"><span lang=\"EN-US\">And the best part of it might be what we now will be able to do throughout this term of office. We now have a database with all the promises the parties made and we can now easily do follow ups and nail down important questions and demand accountability. #kandulova was also nominated for \u00e2\u0080\u009cStora Journalistpriset\u00e2\u0080\u009d which is the Swedish equivalent to the Pulitzer Prize in the category \u00e2\u0080\u009cInnovator of the year\u00e2\u0080\u009d.<\/span><\/p>\n"
    },
    "excerpt": {
        "raw": "",
        "rendered": "Description #kandulova is Mittmedias way of vreating an arena for our audience where they could get the straight answers they always wanted from politicians. It launched about a month before the Swedish elections in 2014 and we collected about 2000 questions from our readers and we also got about 12 000 answers from politicians. Data [&hellip;]"
    },
    "countries": [...],     // Array with ids
    "tags": [...],          // Array with ids
    "topic_tags": [...],    // Array with ids
    "date": "2015-01-30 23:42:38",
    "date_gmt": "2015-01-30 23:42:38",
    "guid": {
        "rendered": "http:\/\/nodabase.net\/?post_type=cases&#038;p=523",
        "raw": "http:\/\/nodabase.net\/?post_type=cases&#038;p=523"
    },
    "case_status": "",
    "case_competition": "not-in-competition",
    "publishers": [...], // Array with publisher objects
    "modified": "2015-01-31 00:38:56",
    "modified_gmt": "2015-01-31 00:38:56",
    "slug": "kandulova",
    "status": "publish",
    "type": "cases",
    "link": "http:\/\/nodabase.net\/cases\/kandulova\/",
    "image": "http:\/\/nodabase.net\/wp-content\/uploads\/2015\/01\/kandu-686x458.jpg"
}
```

#### Publisher object:
```
{
    "title": {
        "raw": "Sydsvenska Dagbladet",
        "rendered": "Sydsvenska Dagbladet"
    },
    "countries": [...] // Array of country ids.
}

```


#### Category object:
```
{
    "id": 86,
    "title": {
        "raw": "Denmark",
        "rendered": "Denmark"
    },
    "slug": "denmark",
    "taxonomy": "publisher-category",
    "description": "",
    "group": "0",
    "parent": "0",
    "count": "4",
    "term_taxonomy_id": "95"
}
```

