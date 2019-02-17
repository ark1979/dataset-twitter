# dataset-twitter
How many Twitter users are in the database?
[
    { "$group" : { "_id" : "$user", } },
    
    { "$count" : "user_count" },
]
Output:

[{'user_count': 659774}]
Who are the most active Twitter users (top ten)?
[
    { "$group" : { "_id" : "$user", "tweet_count" : { "$sum" : 1 }}},
    
    { "$sort" : { "tweet_count" : -1 }},
    
    { "$limit" : 10 },
]
Output:

[{'_id': 'lost_dog', 'tweet_count': 549},
 {'_id': 'webwoke', 'tweet_count': 345},
 {'_id': 'tweetpet', 'tweet_count': 310},
 {'_id': 'SallytheShizzle', 'tweet_count': 281},
 {'_id': 'VioletsCRUK', 'tweet_count': 279},
 {'_id': 'mcraddictal', 'tweet_count': 276},
 {'_id': 'tsarnick', 'tweet_count': 248},
 {'_id': 'what_bugs_u', 'tweet_count': 246},
 {'_id': 'Karen230683', 'tweet_count': 238},
 {'_id': 'DarkPiano', 'tweet_count': 236}]
Who are the five most grumpy (most negative tweets) and the most happy (most positive tweets)?
Most grumpy
[
    { "$group" : { "_id" : "$user", "polarity" : { "$avg" : "$polarity" }}},
    { "$sort" : { "polarity" : 1 }},
    { "$limit" : 5 },
]
Output:

[{'_id': 'fazzwick', 'polarity': 0.0},
 {'_id': 'Charlauren', 'polarity': 0.0},
 {'_id': 'Echo_Park', 'polarity': 0.0},
 {'_id': 'jschenck', 'polarity': 0.0},
 {'_id': 'Iyeman', 'polarity': 0.0}]
Most happy
[
    { "$group" : { "_id" : "$user", "polarity" : { "$avg" : "$polarity" }}},
    { "$sort" : { "polarity" : -1 }},
    { "$limit" : 5 },
]
Output:

[{'_id': 'xoAurixo', 'polarity': 4.0},
 {'_id': 'puchal_ek', 'polarity': 4.0},
 {'_id': 'sdancingsteph', 'polarity': 4.0},
 {'_id': 'EvolveTom', 'polarity': 4.0},
 {'_id': 'LISKFEST', 'polarity': 4.0}]
Which Twitter users link the most to other Twitter users? (Provide the top ten.)
[
    { "$project" : { "text" : { "$split" : [ "$text", " " ] }, "user" : "$user" }},
    { "$unwind" : "$text" },
    { "$match" : { "text" : { "$regex" : "@[a-zA-Z]+" }}},
    { "$group" : { "_id" : "$user", "count" : { "$sum" : 1 }}},
    { "$sort" : { "count" : -1 }},
    { "$limit" : 10 },
]
Output:

[{'_id': 'lost_dog', 'count': 546},
 {'_id': 'dogzero', 'count': 333},
 {'_id': 'tweetpet', 'count': 302},
 {'_id': 'VioletsCRUK', 'count': 296},
 {'_id': 'tsarnick', 'count': 258},
 {'_id': 'what_bugs_u', 'count': 244},
 {'_id': 'Karen230683', 'count': 244},
 {'_id': 'keza34', 'count': 239},
 {'_id': 'SallytheShizzle', 'count': 231},
 {'_id': 'SongoftheOss', 'count': 228}]
Who is are the most mentioned Twitter users? (Provide the top five.)
[
    { "$project" : { "text" : { "$split" : [ "$text", " " ] }, "user" : "$user" }},
    { "$unwind" : "$text" },
    { "$match" : { "text" : { "$regex" : "@[a-zA-Z]+" }}},
    { "$group" : { "_id" : "$text", "mentions" : { "$sum" : 1 }}},
    { "$sort" : { "mentions" : -1 }},
    { "$limit" : 5 },
]
Output:

[{'_id': '@mileycyrus', 'mentions': 4310},
 {'_id': '@tommcfly', 'mentions': 3837},
 {'_id': '@ddlovato', 'mentions': 3349},
 {'_id': '@Jonasbrothers', 'mentions': 1263},
 {'_id': '@DavidArchie', 'mentions': 1222}]
