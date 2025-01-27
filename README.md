## Audible Insight Tool 


### Purpose 
This is a fun little project that I put together on a whim to allow me to analyze my Audible library and wishlist in order to determine the next book for me to read or purchase.

Over time, I've slowly accumulated a large wishlist by looking up the best books on topics I'm interested in and adding them to my wishlist - but then struggling to pick a book to read next, as well as struggling to quickly determine which books are steeply discounted during site-wide sales (e.g. on black friday when every book is 90% off).

Audible, while ultimately reliable and user-friendly for listening to audiobooks, unfortunately makes it difficult to manage or analyze a large library or wishlist. There is no way to export your library or wishlist to a .csv, and it can be remarkably painstaking to click into each book, look at current price and current reviews, and compare with other books in your library or wishlist.

In search of a way to export my library and wishlist to a .csv file that I could review and analyze, I found a free Google Chrome extension that scrapes the information from your browser when you are logged in (and data privacy seems to be respected, with creator's Github available below):
- https://chromewebstore.google.com/detail/audible-library-extractor/deifcolkciolkllaikijldnjeloeaall
- https://github.com/joonaspaakko/audible-library-extractor

However, although you can open the resulting file and sort objectively by price or average rating or other variables, it doesn't quite answer the subjective questions like "What's the best book I can read that will make an impact on me with respect to the time or money I'm going to invest in it", which are important when there are so many great books out there to read or listen to.

Here are a few potential use-cases that I set out to solve:
- Goal: Choose what book to purchase next (from your wishlist)
    - Use-Case: If purchasing a book (especially if there is a sale), prioritize the best deals for your money 
        - Example of Analysis: high priority on low price and/or "good value"
    - Use-Case: If purchasing a book using a credit, prioritize the most expensive books to get the best "bang for your buck"
        - Example of Analysis: high priority on high price

- Goal: Choose the next book to listen to (from your library)
    - Use-Case: If you want to find the best book with the most impact for your time
        - Example of Analysis: high priority on shorter length, highly rated, and highly popular

In short, a tool that will allow one to choose what qualities are most important and then rank the books accordingly.

### How to Use
- Launch the Jupyter Notebook
    - Dependencies: Python, Jupyterlab, Pandas, SciPy, and Re
- Set your variables and priorities and run the first cell
    1. Analysis Type: ['wishlist' or 'library']
    2. File Path: [Paste in the path of the file you wish to analyze]
    3. For length of audiobook, set preference ['long' or 'short'] and priority [0-10]
    4. For rating of audiobook, set minimum for inclusion [0-5] and priority [0-10]
    5. For number of ratings of audiobook, set minimum for inclusion [any number] and priority [0-10]
    6. For price of audiobook (wishlist-only), set preference ['low' or 'high'] and priority [0-10]
    7. For value (lowest cost per hour) of audiobook (wishlist-only), set priority [0-10]
- Review the printed output, fix any errors, adjust weights if desired 
- Run the second cell and review the dataframe
- Export to .csv to see full list
    - Define your file path to ensure it goes to the right place (Downloads folder recommended)
- Open full list in excel or spreadsheet tool with ranked order

### How it Works
1. User sets variables and priorities
    - Tool reads user's file, validates user inputs, and calculates the weights for all variables
    - Tool then generates any errors, or prints variables and calculated weights for review
2. User runs analysis
    - Unneeded fields are dropped (there is a lot of extra metadata that we don't need)
    - Records are dropped where:
        - Format == Podcast
        - Unavailable == True
        - User-defined minimums are not met (for average rating and number of ratings)
    - Minutes in uncalculable format (e.g. 12h 34m) are transformed using regex to just minutes
    - With a fork depending on whether the user is analyzing their wishlist or library (as price is not included in the library export and will break the calculation), every book is assigned a percentile for every qualifying factor (length, rating, price, etc), the weights are applied to each respective percentile value, and the weighted percentile values are all added up to a single priority score (which cannot be greater than 100). If a book has a high ranking across the prioritized factors, it will have a high priority score.
    - Sort by priority score asc and return the df
3. User can export to .csv at a given location

[The jupyter notebook is available for review here.](./python/audible_analysis_public.ipynb)

### Conclusion 
- This is a handy tool that can be used for quick analysis on your Audible library or wishlist.
    - It is particularly helpful if you have a large library or wishlist to analyze. 
    - You can analyze from a variety of factors and preferences, including longer or shorter length, average rating, number of ratings, price, and specifically price-per-listening-hour (time value).
    - A helpful feature is that it utilizes current sale price instead of the "sticker price", so this analysis can be performed anytime a new sale is going on.

In the future, it may be worth creating an interface where a user can upload their file, enter variables and review weights, generate readable list in-browser, and export - which would be easier to use.

Happy reading!