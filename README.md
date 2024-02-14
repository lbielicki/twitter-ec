# Mapreduce twitter analysis- political edition

In this project, I scanned all geotagged tweets sent in 2020 (about 1.1 billion tweets) to look for hashtags #biden or #trump. 
This was based off of my other [twitter-coronavirus](https://github.com/lbielicki/twitter_coronavirus) project. 
Please visit that repo for a more detailed explanation of my process. 


## Project Steps


I modified the mapping file `map.py` to track the country level in addition to the provided language level. 
This step allowed me to look at usage of hashtags in different languages and in different geotagged countries. 
As such, each call to `map.py` resulted in two files, one that ends in `.lang` and one that ends in `.country` for the country dictionary.


Next, I created a shell script called `runmaps.sh` to loop over each file in the dataset and run the `map.py` command on that file. I used `nohup` to run the command even after I disconnected and the `&` operator for parallel processing:
```
$ nohup sh runmaps.sh &
```


Using the resulting `outputs` files, I applied `reduce.py` file to combine all of the `.lang` files into a single file,
and all of the `.country` files into a different file.


I modified the `visualize.py` file to create bar graph of the results and stores the bar graph as a png file.
After running (for example) :

```
$ python3 ./src/visualize.py --input_path=reduced.lang --key='#biden'
```

with the `--input_path` equal to both the country and lang files created in the reduce phase, and the `--key` set to `#biden` and `#trump` in turn, this generated the following plots:

**Use of #biden by country in 2020**

![biden](https://github.com/lbielicki/twitter-ec/blob/master/biden_countryEC.png)

**Use of #trump by country in 2020**

![trump](https://github.com/lbielicki/twitter-ec/blob/master/trump_countryEC.png)

**Step 4: Alternative Reduce**

 I also created a new file alternative_reduce.py. This followed a similar structure to a combined version of the reduce.py and visualize.py files. I used this file to analyze 
 #biden and #trump.

**Use of #biden or #trump in 2020**

![bidentrump](https://github.com/lbielicki/twitter-ec/blob/master/biden_trump.png)

