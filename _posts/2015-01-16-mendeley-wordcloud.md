---
layout: post
title: "Mendeley wordcloud"
tags: [R, Visualization]
---

I recently updated this website, and I then decided to include a wordcloud on
my start page. This wordcloud represents the titles and abstracts of all papers
I've saved in my Mendeley library. Here I will now describe how I made this.

First of all, you need to localize the Mendeley SQLite database. I use a Mac,
so I can find it in
`$HOME/Library/Application\ Support/Mendeley\ Desktop/<email>@www.mendeley.com.sqlite`.
Just replace `<email>` with the email address you use for Mendeley. If you can't
find the database, take a look at [this Mendeley support page](http://support.mendeley.com/customer/portal/articles/227951-how-do-i-locate-mendeley-desktop-database-files-on-my-computer-).
In R, we'll then extract the relevant columns from the database.

{% highlight R %}
library(RSQLite)

con <- dbConnect(RSQLite::SQLite(), "~/Library/Application Support/Mendeley Desktop/<email>@www.mendeley.com.sqlite")
res <- dbSendQuery(con, "SELECT title, abstract FROM Documents")
dbdata <- dbFetch(res)
{% endhighlight %}

Now we have a data frame with two columns containing the title and abstract for
every document in the library. We could create a wordcloud directly from this
data, but it would probably not look very good since it would contain variants
of the same words, i.e. gene and genes or analyzed and analysis. What we want
to do is to find the common denominator among these words, and this
process is called [stemming](http://en.wikipedia.org/wiki/Stemming). I will do
this in R using the [`tm` (text mining) package](http://cran.r-project.org/web/packages/tm/index.html).
`tm` has a data structure called `Corpus`, but in this simple context, I find
it easier to just work with character vectors where each element is a word.

{% highlight R %}
words <- unlist(strsplit(as.character(dbdata), " "))
{% endhighlight %}

To make sure that all words look the same, we transform them to lower case
and remove punctuation and numbers, and finally stem them.

{% highlight R %}
library(tm)
words <- tolower(words)
words <- removePunctuation(words)
words <- removeNumbers(words)
words.stem <- stemDocument(words)
{% endhighlight %}

If we look at these words now, many of them does not make sense. For example,
the words "promote", "promoter", "promotion" and "promoted" would be all stemmed
to the form "promot", which is not a word (as far as I know). To avoid this
gibberish, we can use the `stemCompletion` function. What it does is that it
looks through the stemmed words and transform them to the most common original
variant of the stemmed word. This is the default behavior, and you can look at
the documentation for `stemCompletion` for other options.

{% highlight R %}
words.stem <- stemCompletion(words.stem, dictionary = words)
{% endhighlight %}

Now we have a character vector of words that should make sense. If we would want
to create the wordcloud directly in R, it is possible. However, then you would
want to remove common words such as "the", "of" and "and". This can be done with
the `removeWords` function together with `stopwords` that contains a number of
stop words for a number of languages. The [`wordcloud` package](http://cran.r-project.org/web/packages/wordcloud/)
can then be used to create the actual wordcloud.

In my case I didn't create the wordcloud in R, but used the online service
[Wordle](http://www.wordle.net). Simply save the words to a text file and just
paste it into their service. You can then choose among different color schemes
and layouts to produce a wordcloud that you like.

<img src="/img/mendeley_wordcloud_horizontal_720x323.png" alt="Mendeley wordcloud">
