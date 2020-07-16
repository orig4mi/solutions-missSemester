[Home](README.md)

# Lecture 4: Data Wrangling

1. > Take this [short interactive regex tutorial](https://regexone.com/).
>
Done
2. > Find the number of words (in `/usr/share/dict/words`) that contain at
   > least three `a`s and don't have a `'s` ending. What are the three
   > most common last two letters of those words? `sed`'s `y` command, or
   > the `tr` program, may help you with case insensitivity. How many
   > of those two-letter combinations are there? And for a challenge:
   > which combinations do not occur?
```bash
~$ cat /usr/share/dict/words | sed -n "/.*a.*a.*a.*[^('s)]$/p" | wc -l
~$ cat /usr/share/dict/words | sed -n "/.*a.*a.*a.*[^('s)]$/p" | sed -E 's/^.*(\w\w)$/\1/' | sort | 
    uniq -c | sort -nk1,1 | tail -n3
~$ cat /usr/share/dict/words | sed -n "/.*a.*a.*a.*[^('s)]$/p" | sed -E 's/^.*(\w\w)$/\1/' | sort | 
    uniq -c | sort -nk1,1 | wc -l
``` 
3. > To do in-place substitution it is quite tempting to do something like
   > `sed s/REGEX/SUBSTITUTION/ input.txt > input.txt`. However this is a
   > bad idea, why? Is this particular to `sed`? Use `man sed` to find out
   > how to accomplish this.
```bash
~$ sed -i 's/REGEX/SUBSTITUTION/g' input.txt
```
4. > Find your average, median, and max system boot time over the last ten
   > boots. Use `journalctl` on Linux and `log show` on macOS, and look
   > for log timestamps near the beginning and end of each boot. On Linux,
   > they may look something like:
   > ```
   > Logs begin at ...
   > ```
   > and
   > ```
   > systemd[577]: Startup finished in ...
   > ```
   > On macOS, [look
   > for](https://eclecticlight.co/2018/03/21/macos-unified-log-3-finding-your-way/):
   > ```
   > === system boot:
   > ```
   > and
   > ```
   > Previous shutdown cause: 5
   > ```
   >
   Skipped
5. > Look for boot messages that are _not_ shared between your past three
   > reboots (see `journalctl`'s `-b` flag). Break this task down into
   > multiple steps. First, find a way to get just the logs from the past
   > three boots. There may be an applicable flag on the tool you use to
   > extract the boot logs, or you can use `sed '0,/STRING/d'` to remove
   > all lines previous to one that matches `STRING`. Next, remove any
   > parts of the line that _always_ varies (like the timestamp). Then,
   > de-duplicate the input lines and keep a count of each one (`uniq` is
   > your friend). And finally, eliminate any line whose count is 3 (since
   > it _was_ shared among all the boots).
   >
   Skipped
6. > Find an online data set like [this
   > one](https://stats.wikimedia.org/EN/TablesWikipediaZZ.htm), [this
   > one](https://ucr.fbi.gov/crime-in-the-u.s/2016/crime-in-the-u.s.-2016/topic-pages/tables/table-1).
   > or maybe one [from
   > here](https://www.springboard.com/blog/free-public-data-sets-data-science-project/).
   > Fetch it using `curl` and extract out just two columns of numerical
   > data. If you're fetching HTML data,
   > [`pup`](https://github.com/EricChiang/pup) might be helpful. For JSON
   > data, try [`jq`](https://stedolan.github.io/jq/). Find the min and
   > max of one column in a single command, and the sum of the difference
   > between the two columns in another.
```bash
~$ curl https://ucr.fbi.gov/crime-in-the-u.s.-2016/topic-pages/tables/table-1 -o dataFBI.txt
~$ cat dataFBI.txt | ~bin_compciv/pup 'td:nth-of-type(2) text{}' | awk 'NF' | sed 's/,//g' > column1
~$ cat dataFBI.txt | ~bin_compciv/pup 'td:nth-of-type(4) text{}' | awk 'NF' | sed 's/,//g' > column1
~$ st --max column1
~$ paste column1 column2 | awk '{print $1 - $2}' | st --sum
```
