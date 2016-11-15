Review of Cosmetic Products based on feedback and product reviews on Amazon.


The dataset comprised of 198,503 rows, 10 columns and 1,982,617 data entries. The 'Review Text' is the coloumn that contains the feedback and product review. It is also the main data used, along with the summary, whose corpus is being created and worked on.
This dataset was downloaded in a JSON format and was converted to CSV using python and was then imported into R.

The link provided below will take you to the Vizualization created by the analysis of the data.

https://cdn.rawgit.com/Pushpak168/CosmeticProducts/master/index.html

Preprocessing the file

pre-processing:

reviews<- corpus(musicalinstrumets$reviewText, docnames=musicalinstrumets$ID, docvar=data.frame(summary=musicalinstrumets$summary,overall= musicalinstrumets$summary))

help(tokenize) reviews<- toLower(reviews, keepAcronyms = FALSE) reviewsclean <- tokenize(reviews, removeNumbers=TRUE,
removePunct = TRUE, removeSeparators=TRUE, removeTwitter=FALSE, verbose=TRUE)

dfm.reviews<- dfm(reviewsclean, toLower = TRUE, ignoredFeatures =stopwords("english"), verbose=TRUE, stem=TRUE) head(dfm.reviews)

topfeatures<-topfeatures(dfm.reviews, n=50) topfeatures

compute the table of terms:

term.table <- table(unlist(reviewsclean)) term.table <- sort(term.table, decreasing = TRUE)

read in some stopwords:

stop_words <- stopwords("SMART") stop_words <- c(stop_words, "can", "one", "will", "get", "just", "want","got", "even", "way", "even", "also","like","make","go") stop_words <- tolower(stop_words)

remove terms that are stop words or occur fewer than 5 times:

del <- names(term.table) %in% stop_words | term.table < 5 term.table <- term.table[!del] term.table <- term.table[names(term.table) != ""] vocab <- names(term.table)

now put the documents into the format required by the lda package:

get.terms <- function(x) { index <- match(x, vocab) index <- index[!is.na(index)] rbind(as.integer(index - 1), as.integer(rep(1, length(index)))) } documents <- lapply(reviewsclean, get.terms)

The image below shows the top features in the corpus after cleaning the corpus, tokenizing the corpus and making a dfm. 
