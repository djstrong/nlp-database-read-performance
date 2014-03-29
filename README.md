nlp-database-read-performance
=============================

Every test was preceded by restarting server (in case of Mongo) and dropping disk buffer. All tests were performed using Ruby 1.9.2 (if not specified differently).

```
sync ; sudo sh -c 'echo 3 > /proc/sys/vm/drop_caches'
sudo service mongodb restart
```

Tests:
* All documents - retrieve all documents (~ million)
* 10000 ID documents - randomly generated 10000 IDs with the same seed; 9732 IDs were not present in database

Above tests were also performed with 10 times repetition. It tests cache systems and use of disk buffer. 

| | All documents | 10000 ID documents | 10 times all documents | 10 times 10000 ID documents |
| --- | --- | --- | --- | --- |
| Rod | **0m25.167s** | 0m30.412s | 3m15.944s | 0m36.030s |
| Rod (cache) | 0m37.801s | 0m35.395s | **3m0.352s** | **0m30.426s** |
| MongoMapper 0.12 (IdentityMap) | 2m36.715s | 0m14.351s |  |  |
| MongoMapper 0.13b | 0m51.931s | 0m12.359s | 7m7.040s | 0m48.353s |
| MongoMapper 0.13b (IdentityMap) | 0m47.529s | 0m8.363s | 7m17.493s | 0m44.588s |
| MongoMapper 0.13b (Ruby 2.1)| 0m50.607s | 0m10.465s |  | 1m7.621s |
| MongoMapper 0.13b (Ruby 2.1, IdentityMap)| 0m47.085s | 0m9.714s |  | 1m3.656s |
| Mongo | 0m33.139s | **0m5.793s** | 3m57.887s | 0m42.071s |
| Mongo (Ruby 2.1) | **0m25.711s** | 0m6.143s | 4m4.164s | 0m36.973s |
| Mongoid (Ruby 1.9.3) | 1m10.046s |  |  |  |
| Mongoid (identity map, Ruby 1.9.3) | 1m46.000s |  |  |  |
| Mongoid (Ruby 2.1) | 0m55.921s | 0m6.906s |  | 0m51.628s |

* Tests should be measured in code. 
* Dropping disk buffer command is questionable.
* Documents in Mongo are smaller (less fields).

Mongo performance is comparable to Rod. Mongo is better in random access. ORM for Mongo can be worse twice times than Rod.
