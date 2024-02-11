# EXTRA CREDIT: 49ers Twitter Analysis

I will build upon my previous project where I analyzed tweets from 2020, including the relative frequency of different language tweets, and the frequency of tweets about Coronavirus in the early days of the pandemic.

Inspiried by the upcoming 49ers vs Chiefs superbowl rematch I will track and plot the keyword "49ers" from January 1st to 31th 2020, which was 2 days before the superbowl. It will be interesting to see if the 49ers were increaingly mentioned on twiter leading up to the superbowl which happened on Feburary 2nd. 

I will be using the mapreduce process to analyze the twitter data set.

This is the command for mapping:

```
$ for file in /data/Twitter\ dataset/geoTwitter20-01-*.zip; do 
unzip -p "$file" \
| grep "49ers" \
| sort \
| uniq -c \
| sort -n \
> map.$(basename "$file").dat &
done

```

Now we can run the reducer:

```
cat map.geoTwitter20-01-*.zip.dat | while read line; do
    country_code=$(echo "$line" | sed 's/[^a-zA-Z"]//g')
    counts=$(cat map.* | grep "$country_code" | sed 's/[^0-31]//g')
    sum=$(echo $counts | sed 's/ /+/g' | bc)
    echo "$sum" "$country_code"
done | sort -n > reduce
```
