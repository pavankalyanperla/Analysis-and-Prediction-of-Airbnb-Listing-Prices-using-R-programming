# REPO AUDIT — Analysis and Prediction of Airbnb Listing Prices (R)

**GitHub repo:** https://github.com/pavankalyanperla/Analysis-and-Prediction-of-Airbnb-Listing-Prices-using-R-programming  
**Audit date:** 2026-06-27  
**Auditor:** Claude Code (Sonnet 4.6)

---

## 1. GIT STATUS CHECK

### Local Folder State

**CRITICAL: The local working folder is NOT a git repository.**

Running `git remote -v` and `git log` in `c:\Users\Pavan\OneDrive\Desktop\Analysis and prediction of Airbnb price listings using R\` both returned:

```
fatal: not a git repository (or any of the parent directories): .git
```

There is no `.git` directory locally. The local folder was populated by manual file download/copy — it is **not** a clone of the GitHub repo and has **no shared commit history** with it. The GitHub repo and local folder are effectively disconnected.

Additionally, the local folder's file structure and file names differ from what is on GitHub (detailed in §3 below).

---

### GitHub Repo — Branch and Commits

**Default branch:** `main`  
**Total commits:** 5

| # | Short SHA | Date | Commit Message | What it did |
|---|-----------|------|----------------|-------------|
| 1 | `83ddd79` | 2025-02-24 | Initial commit | Created `README.md` with 2 lines of project description |
| 2 | `6bd91a3` | 2025-02-24 | Add files via upload | Added 3 project files at repo root: the `.docx` report, the `.ipynb` notebook, and `seattle_01.csv.zip`; 5,601 lines added total |
| 3 | `087c075` | 2025-10-18 | Rename seattle_01.csv.zip to dataset/seattle_01.csv.zip | Moved the dataset into a `dataset/` subdirectory |
| 4 | `f468cd1` | 2025-10-18 | Rename Analysis and Prediction of Airbnb Listing Prices.docx to Docs/... | Moved the Word document into a `Docs/` subdirectory |
| 5 | `4976735` | 2025-10-18 | Rename analysis-and-prediction-of-airbnb-listing-prices.ipynb to Script or code/... | Moved the notebook into a `Script or code/` subdirectory |

**Pattern:** Commits 1–2 uploaded raw files on the same day. Commits 3–5 are three GitHub-UI "rename" operations done on the same day ~8 months later to organize files into subfolders. There are no commits with meaningful incremental development history (no feature commits, no fix commits, no iterative improvement).

---

## 2. KERNEL / LANGUAGE CONFIRMATION

The notebook's raw JSON metadata (`metadata.kernelspec` and `metadata.language_info`) was read directly from the local `.ipynb` file. Result:

```json
{
  "kernelspec": {
    "display_name": "R",
    "language": "R",
    "name": "ir"
  },
  "language_info": {
    "codemirror_mode": "r",
    "file_extension": ".r",
    "mimetype": "text/x-r-source",
    "name": "R",
    "pygments_lexer": "r",
    "version": "4.4.0"
  }
}
```

**Verdict: This is genuinely R-via-Jupyter.** The `kernelspec.name` is `"ir"` (the standard IRkernel identifier) and `language_info.name` is `"R"`. GitHub labels the repo "100% Jupyter Notebook" by file extension, which is technically correct (the file is a `.ipynb`), but the execution kernel is R, not Python. All code cells contain native R syntax (confirmed in §4).

### Additional Metadata

The notebook also contains `papermill` metadata:

```json
{
  "papermill": {
    "start_time": "2021-03-19T07:26:11.909217",
    "end_time":   "2021-03-19T07:36:20.501786",
    "duration":   608.592569,
    "version":    "2.3.3"
  }
}
```

And `colab` provenance metadata:

```json
{
  "colab": { "provenance": [] }
}
```

**This means the notebook originated from a Kaggle/Colab environment circa March 2021** (papermill was used to execute it there). The R version at execution time was `4.4.0` (per `language_info`), though installed package outputs reference version `4.4.1` — indicating the notebook was re-run locally in 2024 after the original 2021 Kaggle run. The notebook was then uploaded to GitHub in February 2025.

---

## 3. FULL DIRECTORY TREE

### GitHub Repository Structure (as of latest commit)

```
Analysis-and-Prediction-of-Airbnb-Listing-Prices-using-R-programming/
├── README.md                                                         (tiny, ~2 lines)
├── Docs/
│   └── Analysis and Prediction of Airbnb Listing Prices.docx        (~926 KB)
├── Script or code/
│   └── analysis-and-prediction-of-airbnb-listing-prices.ipynb       (~1.1 MB)
└── dataset/
    └── seattle_01.csv.zip                                            (compressed)
```

### Local Folder Structure

```
c:\Users\Pavan\OneDrive\Desktop\Analysis and prediction of Airbnb price listings using R\
├── Airbnb.csv                                                        (1,835,799 bytes / ~1.75 MB)
├── Analysis and Prediction of Airbnb Listing Prices.ipynb            (1,156,368 bytes / ~1.1 MB)
├── desktop.ini                                                        (152 bytes — Windows thumbnail metadata, should not be committed)
├── R project final report.pdf                                         (1,331,870 bytes / ~1.27 MB)
└── R project report.docx                                              (1,113,718 bytes / ~1.06 MB)
```

### Structural Oddities / Flags

| Issue | Detail |
|-------|--------|
| **Space in folder name** | `Script or code/` — contains spaces AND lowercase mixed with context; awkward for CLI usage (`cd "Script or code"` requires quoting) |
| **Inconsistent casing** | GitHub notebook filename is fully lowercase (`analysis-and-prediction-of-airbnb-listing-prices.ipynb`); local copy uses title case (`Analysis and Prediction of Airbnb Listing Prices.ipynb`) |
| **Dataset name mismatch** | GitHub has `dataset/seattle_01.csv.zip`; notebook loads `read_csv('Airbnb.csv')`. **The notebook will fail to run against the repo dataset as-is.** |
| **Local-only files** | `R project final report.pdf` and `desktop.ini` exist locally but are not in the repo |
| **Zip vs CSV** | GitHub stores the dataset compressed; the notebook reads a flat CSV. An unzip step is undocumented. |
| **Local folder not a git repo** | No `.git` directory — completely disconnected from GitHub history |

---

## 4. NOTEBOOK ANALYSIS

**File:** `Analysis and Prediction of Airbnb Listing Prices.ipynb` (local) / `Script or code/analysis-and-prediction-of-airbnb-listing-prices.ipynb` (GitHub)

### Cell Count Summary

| Metric | Value |
|--------|-------|
| Total cells | 148 |
| Code cells | 68 |
| Markdown cells | 80 |
| Cells with stored error outputs | **0** |
| Cells with plot outputs (base64 images) | ~18 (too large to inline) |
| Empty code cells | 1 (last cell, `11f924d7`) |

Markdown cells are present throughout and are reasonably explanatory. The narrative structure is good — each analysis step is preceded or followed by a markdown cell describing the intent or finding.

---

### All Code Cells (in order)

**Cell 1** — Package installation and loading (Block 1)
```r
install.packages("readr")
library(readr)
install.packages("dplyr")
library(dplyr)
install.packages("ggplot2")
library(ggplot2)
install.packages("tidyr")
library(tidyr)
install.packages("ggcorrplot")
library(ggcorrplot)
install.packages("ggmap")
library(ggmap)

# Replace 'your_api_key_here' with your actual Stadia Maps API key
register_stadiamaps(key = "484128c6-e308-4021-b191-4b8de3e89781")
install.packages("treemapify")
library(treemapify)
```
> **FLAG: Hardcoded Stadia Maps API key (live credential in version-controlled code).**

**Cell 2** — Package installation and loading (Block 2)
```r
install.packages("tidyverse")
install.packages("caret")
install.packages("randomForest")
install.packages("rpart")

library(tidyverse)
library(caret)
library(randomForest)
library(rpart)
```
> Note: `tidyverse` is loaded here after `readr`, `dplyr`, `ggplot2`, `tidyr` were already individually loaded in Cell 1 — redundant. `randomForest` and `rpart` are never used for modeling (see §9).

**Cell 3** — Data import
```r
airbnb <- read_csv('Airbnb.csv')
```
> Relative path. Works locally but **will not work against the repo** (which has `dataset/seattle_01.csv.zip`).

**Cell 4** — Dimension and structure
```r
dim(airbnb)
str(airbnb)
```

**Cell 5** — Convert to data.frame
```r
airbnb <- as.data.frame(airbnb)
class(airbnb)
```

**Cell 6** — Count total NAs
```r
sum(is.na(airbnb))
```

**Cell 7** — Count NAs per column
```r
colSums(is.na(airbnb))
```

**Cell 8** — Build NA visualization dataframe
```r
na_vis <- data.frame(t(colSums(is.na(airbnb))))
na_bar <- data.frame(Features = names(na_vis), totals = colSums(na_vis))
```

**Cell 9** — Plot NA distribution
```r
na_bar %>% ggplot(aes(x = reorder(Features, totals), y = totals, fill = Features, label = totals)) +
  geom_bar(stat = "identity") +
  ggtitle("NA Distribution") +
  xlab("Features") +
  ylab("Total NAs") +
  coord_flip() +
  geom_text(size = 3, position = position_stack(vjust = 0.5)) +
  theme_bw() +
  theme(plot.title = element_text(hjust = 0.5))
```

**Cell 10** — Impute `overall_satisfaction` NAs with mean
```r
airbnb$overall_satisfaction[is.na(airbnb$overall_satisfaction)] <- mean(airbnb$overall_satisfaction, na.rm = TRUE)
```
> **FLAG: Imputation performed on full dataset before train/test split — minor data leakage (see §10).**

**Cell 11** — Confirm imputed mean
```r
mean(airbnb$overall_satisfaction)
```
*Output: `[1] 4.841226`*

**Cell 12** — Inspect imputed values
```r
head(airbnb$overall_satisfaction)
```
*Output: `[1] 5.000000 4.841226 4.500000 5.000000 4.500000 4.500000`*

**Cell 13** — Impute `bathrooms` NAs with 0
```r
airbnb <- airbnb %>% replace_na(list(bathrooms = 0))
```

**Cell 14** — Confirm zero NAs remain
```r
sum(is.na(airbnb))
```
*Output: `[1] 0`*

**Cell 15** — Inspect column names
```r
names(airbnb)
```

**Cell 16** — Inspect index column
```r
head(airbnb$...1)
```

**Cell 17** — Inspect ID columns
```r
head(airbnb$room_id)
head(airbnb$host_id)
```

**Cell 18** — Inspect distinct address values
```r
airbnb %>% select(address) %>% distinct()
```
*Output: 27 unique address strings with inconsistent formatting*

**Cell 19** — Address cleaning (nested gsub, 26 levels deep)
```r
address_clean <- gsub("Seattle, WA, United States", "Seattle",
  gsub("Kirkland, WA, United States", "Kirkland",
  gsub("Bellevue, WA, United States", "Bellevue",
  gsub("Redmond, WA, United States", "Redmond",
  gsub("Mercer Island, WA, United States", "Mercer Island",
  gsub("Seattle, WA", "Seattle",
  gsub("Renton, WA, United States", "Renton",
  gsub("Ballard, Seattle, WA, United States", "Seattle",
  gsub("West Seattle, WA, United States", "Seattle",
  gsub("Medina, WA, United States", "Medina",
  gsub("Newcastle, WA, United States", "Newcastle",
  gsub("Seattle , WA, United States", "Seattle",
  gsub("Ballard Seattle, WA, United States", "Seattle",
  gsub("Yarrow Point, WA, United States", "Yarrow Point",
  gsub("Clyde Hill, WA, United States", "Clyde Hill",
  gsub("Tukwila, WA, United States", "Tukwila",
  gsub("Seattle, Washington, US, WA, United States", "Seattle",
  gsub("Capitol Hill, Seattle, WA, United States", "Seattle",
  gsub("Kirkland , Wa, United States", "Kirkland",
  gsub("Hunts Point, WA, United States", "Hunts Point",
  gsub("Seattle, DC, United States", "Seattle",
  gsub("Seattle, United States", "Seattle",
  gsub("Vashon, WA, United States", "Vashon",
  gsub("Kirkland , WA, United States", "Kirkland",
  gsub("Bothell, WA, United States", "Bothell",
  gsub("Washington, WA, United States", "Seattle",
      airbnb$address))))))))))))))))))))))))))
```

**Cell 20** — Clean Chinese-language Seattle entry via regex
```r
address_clean2 <- gsub(".*WA*.", "Seattle", address_clean)
```

**Cell 21** — Reassign cleaned address column
```r
airbnb$address <- gsub("Seattle, United States", "Seattle",
                 gsub("Seattle United States", "Seattle", address_clean2))
```

**Cell 22** — Summarize listings by city
```r
city_list <- airbnb %>% group_by(address) %>% summarize(listing_sum = n()) %>%
  arrange(-listing_sum)
city_list
```
*Output: 14 cities, Seattle leads with 6,791 listings*

**Cell 23** — Bar chart: Location distribution
```r
city_list %>%
  ggplot(aes(x = reorder(address, listing_sum), y = listing_sum,
             fill = address, label = listing_sum)) +
  geom_bar(stat = "identity") +
  ggtitle("Location Distribution") +
  xlab("Location") +
  ylab("Total Listings") +
  coord_flip() +
  geom_text(size = 4, position = position_stack(vjust = 0.5)) +
  theme_bw() +
  theme(plot.title = element_text(hjust = 0.5))
```

**Cell 24** — Inspect `last_modified`
```r
head(airbnb$last_modified)
```

**Cell 25** — Inspect `location` (WKB hex strings)
```r
head(airbnb$location)
```

**Cell 26** — Inspect listing names
```r
head(airbnb$name)
```

**Cell 27** — Check currency uniqueness
```r
airbnb %>% select(currency) %>% distinct()
```
*Output: only `"USD"`*

**Cell 28** — Check rate_type uniqueness
```r
airbnb %>% select(rate_type) %>% distinct()
```
*Output: only `"nightly"`*

**Cell 29** — Drop irrelevant columns and rename
```r
airbnb <- airbnb %>% select(-c(...1, room_id, host_id, last_modified,
                              location, name, currency, rate_type)) %>%
                    rename(city = address, rating = overall_satisfaction,
                           reviews_sum = reviews)
```

**Cell 30** — Confirm column count
```r
ncol(airbnb)
```
*Output: `[1] 10`*

**Cell 31** — Print column names
```r
colnames(airbnb)
```
*Output: `"room_type" "city" "reviews_sum" "rating" "accommodates" "bedrooms" "bathrooms" "price" "latitude" "longitude"`*

**Cell 32** — Reorder columns
```r
airbnb <- airbnb[,c(8, 2, 4, 3, 1, 6, 7, 5, 9, 10)]
```

**Cell 33** — Confirm column order
```r
names(airbnb)
```
*Output: `"price" "city" "rating" "reviews_sum" "room_type" "bedrooms" "bathrooms" "accommodates" "latitude" "longitude"`*

**Cell 34** — Inspect cleaned dataset
```r
head(airbnb)
```

**Cell 35** — Select numeric columns for correlation
```r
airbnb_num <- airbnb %>% select(-c(city, room_type))
```

**Cell 36** — Build correlation matrix
```r
airbnb_cor <- cor(airbnb_num)
```

**Cell 37** — Plot correlogram
```r
ggcorrplot(airbnb_cor) +
  labs(title = "Airbnb Correlogram") +
  theme(plot.title = element_text(hjust = 0.5))
```

**Cell 38** — Price density plot (≤$300)
```r
airbnb %>% filter(price <= 300) %>% ggplot(aes(price)) +
  geom_density(fill = "deepskyblue", size = 1.5, color = "navyblue", alpha = 0.5) +
  xlab("Price") +
  ylab("Density") +
  ggtitle("Price Distribution at or Below $300") +
  theme(plot.title = element_text(hjust = 0.5))
```

**Cell 39** — Download Stadia base map
```r
seattle_map <- get_stadiamap(bbox = c(left = -122.5, bottom = 47.49,
                              right = -122.09, top = 47.74), zoom = 9,
                                maptype = "stamen_toner")
```

**Cell 40** — Price summary statistics
```r
summary(airbnb$price)
```
*Output: Min=15, Q1=65, Median=88, Mean=113, Q3=125, Max=5900*

**Cell 41** — Quantile + percentage ≤$300
```r
quantile(airbnb$price)
sum(airbnb$price <= 300) / length(airbnb$price)
```
*Output: 96.5% of listings priced ≤$300*

**Cell 42** — Filter for map subset
```r
airbnb_map <- airbnb %>% filter(price <= 300)
```

**Cell 43** — Geographic heatmap (all prices)
```r
ggmap(seattle_map, extent = "normal") +
  geom_point(data = airbnb,
             aes(x = longitude, y = latitude, color = price),
             size = 1.5, alpha = .6) +
  scale_color_gradientn(colors = c("mediumblue", "lawngreen", "blueviolet", "red"),
             values = scales::rescale(c(.003, .013, .0176, .025, .2, .3, .4))) +
  xlab("Longitude") +
  ylab("Latitude") +
  ggtitle("Seattle Location Pricing") +
  theme(plot.title = element_text(hjust = 0.5),
        panel.border = element_rect(color = "gray", fill = NA, size = 3))
```

**Cell 44** — Geographic heatmap (≤$300)
```r
ggmap(seattle_map, extent = "normal") +
  geom_point(data = airbnb_map,
             aes(x = longitude, y = latitude, color = price),
             size = 1.5, alpha = .6) +
  scale_color_gradientn(colours = rainbow(5)) +
  xlab("Longitude") +
  ylab("Latitude") +
  ggtitle("Seattle Pricing at or Below $300") +
  theme(plot.title = element_text(hjust = 0.5),
        panel.border = element_rect(color = "gray", fill = NA, size = 3))
```

**Cell 45** — Percent in $50–$150 range
```r
sum(airbnb$price >= 50 & airbnb$price <= 150) / length(airbnb$price)
```
*Output: `[1] 0.6998416` — 70% of listings*

**Cell 46** — Treemap: Listings by city
```r
city_distribution <- airbnb %>% group_by(city) %>% summarize(listing_sum = n()) %>%
  arrange(-listing_sum)
city_distribution <- city_distribution %>%
  unite("tmlab", city:listing_sum, sep = " ", remove = FALSE)
city_distribution %>% ggplot(aes(area = listing_sum, fill = city, label = tmlab)) +
  geom_treemap() +
  geom_treemap_text(fontface = "italic", col = "white", place = "center", grow = TRUE)
```

**Cell 47** — Mean price by city
```r
city_price <- airbnb %>% group_by(city) %>%
  summarize(mean_price = mean(price), listing_sum = n()) %>%
  arrange(-mean_price) %>% mutate(mean_price = sprintf("%0.1f", mean_price))
city_price$mean_price <- as.integer(city_price$mean_price)
```

**Cell 48** — Bar chart: Mean price per city
```r
city_price %>% ggplot(aes(x = reorder(city, mean_price), y = mean_price,
                         fill = city, label = mean_price)) +
  geom_bar(stat = "identity") +
  coord_flip() +
  xlab("City") +
  ylab("Mean Price") +
  ggtitle("Mean Price per Night by City") +
  geom_text(size = 4, position = position_stack(vjust = 0.5)) +
  theme_bw() +
  theme(plot.title = element_text(hjust = 0.5))
```

**Cell 49** — Add percentage column to city comparison
```r
city_comp <- city_price %>%
  mutate(percentage = sprintf("%0.3f", (listing_sum / sum(listing_sum) * 100)))
city_comp$percentage <- paste(city_comp$percentage, "%")
city_comp <- city_comp %>%
  unite("citylab", mean_price, percentage, sep = ", ", remove = FALSE)
```

**Cell 50** — Bar chart: Mean price + listing % by city
```r
city_comp %>%
  ggplot(aes(x = reorder(city, mean_price), y = mean_price,
             fill = city, label = citylab)) +
  geom_bar(stat = "identity") +
  coord_flip() +
  xlab("City") +
  ylab("Mean Price") +
  ggtitle("Mean Price & Percentage of Total Listings by City") +
  geom_text(size = 3, position = position_stack(vjust = 0.5)) +
  theme_bw() +
  theme(plot.title = element_text(hjust = 0.5))
```

**Cell 51** — Rating vs price dual-axis chart
```r
rating_comp <- airbnb %>% group_by(city) %>%
  summarize(mean_rating = mean(rating), mean_price = mean(price)) %>%
  select(city, mean_rating, mean_price)

ylim_1 <- c(0, 10)
ylim_2 <- c(70, 400)
b <- diff(ylim_1) / diff(ylim_2)
a <- b * (ylim_1[1] - ylim_2[1])

ggplot(rating_comp, aes(city, group = 1)) +
  geom_bar(aes(y = mean_rating), stat = "identity", color = "navyblue", alpha = .7) +
  geom_line(aes(y = a + mean_price * b), color = "red", size = 2) +
  scale_y_continuous(name = "Mean Rating",
                     sec.axis = sec_axis(~ (. - a) / b, name = "Mean Price")) +
  xlab("City") +
  ggtitle("Rating & Price Comparison by City") +
  theme(axis.text.x = element_text(angle = 45, hjust = 1),
        plot.title = element_text(hjust = 0.5))
```

**Cell 52** — Rating range
```r
range(rating_comp$mean_rating)
```
*Output: `[1] 4.795613 5.000000`*

**Cell 53** — Reviews vs price scatter
```r
airbnb %>% filter(price <= 1000) %>%
  ggplot(aes(x = reviews_sum, y = price)) +
  geom_point(color = "navyblue", alpha = 0.6, size = 1.5) +
  xlab("Number of Reviews") +
  ylab("Price") +
  ggtitle("Review vs Price Distribution") +
  theme_bw() +
  theme(plot.title = element_text(hjust = 0.5))
```

**Cell 54** — Room type mean price
```r
airbnb %>% group_by(room_type) %>%
  summarize(mean_price = mean(price)) %>%
  ggplot(aes(reorder(room_type, mean_price),
             y = mean_price, label = sprintf("%0.2f", round(mean_price, digits = 2)))) +
  geom_bar(stat = "identity", color = "navyblue", size = 1.5, fill = "deepskyblue") +
  coord_flip() +
  xlab("Room Type") +
  ylab("Mean Price") +
  ggtitle("Mean Price by Room Type") +
  geom_text(size = 3, position = position_stack(vjust = 0.5)) +
  theme_bw() +
  theme(plot.title = element_text(hjust = 0.5))
```

**Cell 55** — Bathrooms mean price bar
```r
airbnb %>% group_by(bathrooms) %>% summarize(mean_price = mean(price)) %>%
  ggplot(aes(bathrooms, mean_price)) +
  geom_bar(stat = "identity", fill = "deepskyblue", color = "navyblue", size = 1.2) +
  xlab("Number of Bathrooms") +
  ylab("Mean Price per Night") +
  ggtitle("Mean Price per Number of Bathrooms") +
  theme_bw() +
  theme(plot.title = element_text(hjust = 0.5))
```

**Cell 56** — Bathrooms listing count bar
```r
airbnb %>% group_by(bathrooms) %>% summarize(sum_bath = length(bathrooms)) %>%
  ggplot(aes(reorder(bathrooms, sum_bath), y = sum_bath, label = sum_bath)) +
  geom_bar(stat = "identity", fill = "deepskyblue", color = "cyan", size = 1.2) +
  coord_flip() +
  geom_text(size = 5, color = "navyblue", position = position_stack(vjust = 0.5)) +
  xlab("Number of Bathrooms") +
  ylab("Total Number of Listings") +
  ggtitle("Listing Distribution by Bathroom Total") +
  theme_bw() +
  theme(plot.title = element_text(hjust = 0.5))
```

**Cell 57** — Bedrooms mean price bar
```r
airbnb %>% group_by(bedrooms) %>%
  summarize(mean_price = mean(price)) %>%
  ggplot(aes(bedrooms, mean_price)) +
  geom_bar(stat = "identity", color = "navyblue", fill = "deepskyblue", size = 1.5) +
  xlab("Number of Bedrooms") +
  ylab("Mean Price per Night") +
  ggtitle("Mean Price Distribution by Bedroom Total") +
  theme_bw() +
  theme(plot.title = element_text(hjust = 0.5))
```

**Cell 58** — Bedrooms listing count bar
```r
airbnb %>% group_by(bedrooms) %>% summarize(sum_beds = length(bedrooms)) %>%
  ggplot(aes(reorder(bedrooms, sum_beds), y = sum_beds, label = sum_beds)) +
  geom_bar(stat = "identity", color = "cyan", fill = "deepskyblue", size = 1.5) +
  coord_flip() +
  geom_text(size = 5, color = "navyblue", position = position_stack(vjust = 0.5)) +
  xlab("Total Number of Listings") +
  ylab("Listing Distribution by Bedroom Total") +
  ggtitle("Number of Bedrooms") +
  theme_bw() +
  theme(plot.title = element_text(hjust = 0.5))
```

**Cell 59** — Accommodates mean price bar
```r
airbnb %>% group_by(accommodates) %>% summarize(mean_price = mean(price)) %>%
  ggplot(aes(accommodates, mean_price)) +
  geom_bar(stat = "identity", color = "navyblue", size = 2, fill = "deepskyblue") +
  xlab("Accommodates") +
  ylab("Mean Price") +
  ggtitle("Number of Guests Accommodated & Mean Price") +
  theme_bw() +
  theme(plot.title = element_text(hjust = 0.5))
```

**Cell 60** — Accommodates listing count bar
```r
airbnb %>% group_by(accommodates) %>%
  summarize(sum_acc = length(accommodates)) %>%
  ggplot(aes(x = factor(accommodates), y = sum_acc)) +
  geom_bar(stat = "identity", color = "navyblue", fill = "deepskyblue", size = 1.5) +
  xlab("Number of Guests Accommodated") +
  ylab("Total Number of Listings") +
  ggtitle("Listings Sum by Accommodation Total") +
  theme_bw() +
  theme(plot.title = element_text(hjust = 0.5))
```

**Cell 61** — Create test split (10%)
```r
set.seed(123, sample.kind = "Rounding")
test_index <- createDataPartition(y = airbnb$price, times = 1, p = 0.1, list = F)
airbnb_combined <- airbnb[-test_index,]
airbnb_test <- airbnb[test_index,]
rm(test_index)
```
*Warning: `"non-uniform 'Rounding' sampler used"` — deprecation warning in R ≥ 3.6*

**Cell 62** — Create train/validation split (80/20 of combined)
```r
set.seed(123)
test_index <- createDataPartition(y = airbnb_combined$price, times = 1,
                                  p = 0.2, list = FALSE)
airbnb_train <- airbnb_combined[-test_index,]
validation <- airbnb_combined[test_index,]
rm(test_index)
```

**Cell 63** — Define RMSE function
```r
RMSE <- function(true_ratings, predicted_ratings) {
  sqrt(mean((true_ratings - predicted_ratings)^2))
}
```

**Cell 64** — Baseline median
```r
airbnb_train_median <- median(airbnb_train$price)
```

**Cell 65** — Baseline RMSE
```r
MM_RMSE <- RMSE(validation$price, airbnb_train_median)
results_table <- tibble(Model_Type = "Baseline Median",
                        RMSE = MM_RMSE) %>%
  mutate(RMSE = sprintf("%0.2f", RMSE))
knitr::kable(results_table)
```
*Output:*
```
|Model_Type      |RMSE  |
|:---------------|:-----|
|Baseline Median |99.05 |
```

**Cell 66** — Define regression formula
```r
airbnb_form <- price ~ rating + reviews_sum + bedrooms + bathrooms +
  accommodates + latitude + longitude
```
> Note: `room_type` and `city` (both categorical, both explored in EDA) are **excluded** from this formula.

**Cell 67** — Fit linear model and evaluate
```r
lm_airbnb <- lm(airbnb_form, data = airbnb_train)

lm_preds <- predict(lm_airbnb, validation)

LM_RMSE <- RMSE(validation$price, lm_preds)
results_table <- tibble(Model_Type = c("Baseline Median", "Linear"),
                        RMSE = c(MM_RMSE, LM_RMSE)) %>%
  mutate(RMSE = sprintf("%0.2f", RMSE))

knitr::kable(results_table)
```
*Output:*
```
|Model_Type      |RMSE  |
|:---------------|:-----|
|Baseline Median |99.05 |
|Linear          |73.80 |
```
> `summary(lm_airbnb)` is **never called** — R², Adj. R², coefficient p-values, and F-statistic are completely absent from stored outputs.

**Cell 68** — Empty code cell (last cell)
```r
(empty)
```

---

### Language Used in Code Cells

All 67 non-empty code cells use R syntax exclusively:
- Assignment operator `<-` throughout
- `library()` / `install.packages()`
- Pipe operator `%>%` (magrittr/dplyr)
- `ggplot2` grammar (`ggplot()`, `geom_bar()`, `geom_density()`, `geom_point()`, `aes()`, `theme_bw()`)
- `dplyr` verbs: `select()`, `filter()`, `group_by()`, `summarize()`, `mutate()`, `arrange()`, `rename()`
- `tidyr`: `replace_na()`, `unite()`
- R-style data structures: `data.frame()`, `tibble()`, `as.integer()`, `sprintf()`
- R model syntax: `lm()`, `predict()`, `createDataPartition()`

**No Python syntax anywhere.** Consistent with `kernelspec.name = "ir"`.

### Cells with Stored Error Outputs

**Zero error cells.** Several cells have warnings (package version warnings, the `sample.kind` deprecation warning), but no cells have `"output_type": "error"` in their stored outputs.

### Narrative Coverage

Markdown cells are present before and after most code cells. The notebook is reasonably well-narrated — each major section has explanatory text. However, some markdown cells are sparse or contain placeholder text (e.g., cell `radio-copper` contains an orphaned text block that reads like an LLM self-dialogue: *"It seems like there might be a slight confusion in your request..."*).

---

## 5. OTHER FILES

### `Airbnb.csv` (Local Only)

**Size:** 1,835,799 bytes (~1.75 MB)  
**Rows:** 7,576 data rows (confirmed from `str()` output)  
**Columns (18):**

| # | Column Name | Type | Notes |
|---|-------------|------|-------|
| 1 | *(unnamed index)* | int | Row index 0–7575 |
| 2 | room_id | num | Unique listing ID |
| 3 | host_id | num | Unique host ID |
| 4 | room_type | chr | "Entire home/apt", "Private room", "Shared room" |
| 5 | address | chr | City/region string (inconsistent format) |
| 6 | reviews | num | Total review count |
| 7 | overall_satisfaction | num | Rating 0–5; **1,473 NAs** |
| 8 | accommodates | num | Max guest count |
| 9 | bedrooms | num | Bedroom count; some 0-bedroom studios |
| 10 | bathrooms | num | Bathroom count in 0.5 increments; **2 NAs** |
| 11 | price | num | Nightly price in USD; range $15–$5,900 |
| 12 | last_modified | datetime | Last listing update timestamp; all ~2018-12-20 |
| 13 | latitude | num | WGS84 latitude |
| 14 | longitude | num | WGS84 longitude |
| 15 | location | chr | WKB hex geometry (redundant with lat/lon) |
| 16 | name | chr | Free-text listing name |
| 17 | currency | chr | Always "USD" |
| 18 | rate_type | chr | Always "nightly" |

**First 3 data rows:**
```
row 0: room_id=2318, Entire home/apt, Seattle WA US, 21 reviews, rating=5.0, acc=8, beds=4, bath=2.5, price=250
row 1: room_id=3335, Entire home/apt, Seattle WA US, 1 review, rating=NA, acc=4, beds=2, bath=1.0, price=100
row 2: room_id=4291, Private room, Seattle WA US, 63 reviews, rating=4.5, acc=2, beds=1, bath=1.0, price=82
```

**Note:** The dataset appears to be a Seattle Airbnb scrape from **December 2018**. On GitHub, this file is stored as `dataset/seattle_01.csv.zip` — a different name and in compressed format.

---

### `R project final report.pdf` (Local Only)

**Size:** 1,331,870 bytes (~1.27 MB)  
Not in GitHub repo. Likely an exported version of the project report. Content not read (binary PDF).

---

### `R project report.docx` (Local Only) / `Docs/Analysis and Prediction of Airbnb Listing Prices.docx` (GitHub)

**Local size:** 1,113,718 bytes (~1.06 MB)  
**GitHub size:** ~926 KB  
These may be different versions of the same document (local is larger). Not text-readable.

---

### `desktop.ini` (Local Only)

**Size:** 152 bytes  
Windows thumbnail/folder metadata. Should never be committed to a repository.

---

## 6. EXISTING README.md

The README exists **on GitHub only** (not in the local folder). Full content verbatim:

```
# Analysis-and-Prediction-of-Airbnb-Listing-Prices-using-R-programming
Analyzes Airbnb listing prices using R. It includes data cleaning, EDA, feature engineering, and regression modeling to predict prices. Key factors influencing pricing through visualizations and model evaluation, improving price estimation accuracy.
```

That is the entirety of the README — a title (auto-generated repo name) and two sentences. There are no:
- Setup instructions
- Dependency list
- How to run the notebook
- Dataset source or description
- Results summary
- Badges or visuals

---

## 7. R-SPECIFIC DEPENDENCY CHECK

### Dependency Manifest

| File | Present? |
|------|----------|
| `renv.lock` | **ABSENT** |
| `packrat/` directory | **ABSENT** |
| `DESCRIPTION` file | **ABSENT** |

No reproducibility manifest of any kind exists. The R version and exact package versions are only inferrable from stored notebook outputs.

---

### All `library()` / `require()` Calls (in order of first appearance)

| Order | Package | Purpose |
|-------|---------|---------|
| 1 | `readr` | CSV import (`read_csv`) |
| 2 | `dplyr` | Data manipulation (`select`, `filter`, `group_by`, `mutate`, `rename`, `arrange`) |
| 3 | `ggplot2` | All visualizations |
| 4 | `tidyr` | NA handling (`replace_na`), column combining (`unite`) |
| 5 | `ggcorrplot` | Correlation matrix plot |
| 6 | `ggmap` | Stadia Maps geographic overlay |
| 7 | `treemapify` | Treemap visualization |
| 8 | `tidyverse` | Meta-package (redundant — readr, dplyr, ggplot2, tidyr already loaded above) |
| 9 | `caret` | `createDataPartition` for train/test splitting |
| 10 | `randomForest` | **Loaded but never used** for modeling |
| 11 | `rpart` | **Loaded but never used** for modeling |

---

### R Version

- `language_info.version`: **`4.4.0`** (from kernel metadata — Kaggle/Colab run, March 2021)
- Package installation outputs reference **`4.4.1`** (local re-run in 2024)
- Packages show `"Warning message: package 'X' was built under R version 4.4.1"` for all packages — current local R is likely **4.4.1 or 4.4.2**

---

## 8. MISSING STANDARD FILES CHECK

| File | Present Locally | Present on GitHub | Notes |
|------|----------------|-------------------|-------|
| `.gitignore` | **ABSENT** | **ABSENT** | No git anywhere; `desktop.ini` and `.DS_Store` would be committed by accident |
| `README.md` | **ABSENT** | Present (minimal) | Local folder has no README |
| `renv.lock` | **ABSENT** | **ABSENT** | No reproducibility manifest |
| `requirements.txt` | **ABSENT** | **ABSENT** | N/A for R (would be `renv.lock`) |
| `LICENSE` | **ABSENT** | **ABSENT** | No license declared |
| `DESCRIPTION` | **ABSENT** | **ABSENT** | Not an R package, so not strictly required, but renv uses it |

---

## 9. MODEL & METRICS INVENTORY

### Models Fit vs. Libraries Loaded

| Package/Function | Status | Notes |
|-----------------|--------|-------|
| `lm()` (linear regression) | **FITTED** | Cell 67 |
| `randomForest()` | **NOT FITTED** | `library(randomForest)` loaded in Cell 2; no `randomForest()` call anywhere |
| `rpart()` (decision tree) | **NOT FITTED** | `library(rpart)` loaded in Cell 2; no `rpart()` call anywhere |
| `glm()` | **NOT USED** | Not loaded or called |

### Models Actually Fit

#### Model 1: Baseline Median

A naïve baseline that predicts the training-set median price for every observation.

- **Evaluated on:** validation set
- **RMSE:** `99.05`

#### Model 2: Linear Regression (`lm`)

```r
airbnb_form <- price ~ rating + reviews_sum + bedrooms + bathrooms +
  accommodates + latitude + longitude
lm_airbnb <- lm(airbnb_form, data = airbnb_train)
```

- **Evaluated on:** validation set
- **RMSE:** `73.80`
- **Improvement over baseline:** 25.5% RMSE reduction

**Metrics present in stored output:**

| Metric | Value | Present? |
|--------|-------|---------|
| RMSE (validation) | 73.80 | YES |
| R² | — | **NO** (`summary(lm_airbnb)` never called) |
| Adjusted R² | — | **NO** |
| Coefficient p-values | — | **NO** |
| F-statistic | — | **NO** |
| MAE | — | **NO** |
| Test set RMSE | — | **NO** (test set `airbnb_test` never evaluated) |

### Train/Test/Validation Split

```
Full dataset: 7,576 rows
  └─ Step 1: createDataPartition(p=0.1)
        ├─ airbnb_test (10%, ~758 rows) — CREATED, NEVER EVALUATED
        └─ airbnb_combined (90%, ~6,818 rows)
              └─ Step 2: createDataPartition(p=0.2)
                    ├─ validation (20%, ~1,364 rows) — used for RMSE
                    └─ airbnb_train (80%, ~5,454 rows) — used for training
```

**The test set `airbnb_test` is created but never referenced again after `rm(test_index)` in Cell 61.** Only the validation set RMSE is reported. This means there is no truly held-out final evaluation — the validation set RMSE is the only performance metric presented.

### Model Assumption Checks

| Check | Present? |
|-------|---------|
| Residual plots (`plot(lm_airbnb)`) | **ABSENT** |
| Normality of residuals (Shapiro-Wilk or Q-Q plot) | **ABSENT** |
| Multicollinearity / VIF (`vif()`) | **ABSENT** |
| Homoscedasticity check | **ABSENT** |
| Leverage/influence analysis | **ABSENT** |

No model assumption diagnostics are performed.

---

## 10. CODE QUALITY OBSERVATIONS

### Hardcoded Credentials

**Cell 1, line 14:**
```r
register_stadiamaps(key = "484128c6-e308-4021-b191-4b8de3e89781")
```
A live Stadia Maps API key is hardcoded in the notebook. This key is now publicly visible in the GitHub repo. It should be revoked and moved to an environment variable or `.Renviron` file.

### Hardcoded / Non-portable Paths

```r
airbnb <- read_csv('Airbnb.csv')   # Cell 3
```
This is a relative path, which is acceptable for notebook-style scripts when the working directory is set correctly. However, the notebook contains no `setwd()` call, and the file is named `Airbnb.csv` locally while the repo stores `seattle_01.csv.zip`. **The notebook cannot be run from the repo as-is.**

No Windows-style absolute paths (`C:\...`) were found.

### Unused Library Calls

- `library(randomForest)` — loaded, zero usage
- `library(rpart)` — loaded, zero usage
- `library(tidyverse)` — redundant (duplicates `readr`, `dplyr`, `ggplot2`, `tidyr` already loaded individually above it)

### Deprecated Syntax / Functions

- `set.seed(123, sample.kind = "Rounding")` in Cell 61 generates:
  ```
  Warning message: "non-uniform 'Rounding' sampler used"
  ```
  The `sample.kind` parameter was deprecated in R ≥ 3.6 when the default RNG method changed. The recommended fix is to simply use `set.seed(123)`.

- `geom_bar(..., size = 1.5)` and `geom_line(..., size = 2)` — in `ggplot2` ≥ 3.4.0, `size` for line/border aesthetics was replaced by `linewidth`. These will generate deprecation warnings on current ggplot2 versions.

- `panel.border = element_rect(color = "gray", fill = NA, size = 3)` in map plots — same `size` → `linewidth` issue.

### Orphaned/Dead Markdown Cell

Cell `radio-copper` contains:
> *"It seems like there might be a slight confusion in your request. You mentioned exploring the relationship between mean price per listing and the number of bedrooms, but then referred to beds. Could you please confirm whether you meant to explore..."*

This is an AI-generated self-clarification that was accidentally left as a markdown cell. It reads as a prompt/response artifact and should be removed.

### Inconsistent `set.seed` Usage

- Cell 61: `set.seed(123, sample.kind = "Rounding")` — uses deprecated argument
- Cell 62: `set.seed(123)` — clean form

Both use seed 123, but the inconsistency is unnecessary noise.

### Plot Labeling

All ggplot2 charts have `ggtitle()`, `xlab()`, and `ylab()` set. Center-aligned titles via `theme(plot.title = element_text(hjust = 0.5))` are used consistently. No legends are missing for fill-mapped plots. Plot quality is good.

**Plotting library used: exclusively `ggplot2`** (plus `ggmap` for geographic overlays, `ggcorrplot` for the correlation matrix, `treemapify` for the treemap). No base R `plot()` calls.

### Data Leakage Risk

- **NA imputation (Cell 10):** `mean(airbnb$overall_satisfaction, na.rm=TRUE)` is computed on the full 7,576-row dataset. The imputed mean therefore incorporates rows that will end up in the test set. This is technically data leakage, though the practical effect is negligible (mean of a narrow distribution: range 4.80–5.00, with the 1,473 NAs affecting a small proportion of the full feature range).
- **No scaling or normalization** is applied, so no leakage through standardization.
- **No encoding of categorical variables** before modeling — `room_type` and `city` are simply excluded from the model formula rather than being encoded (e.g., dummy variables via `model.matrix`).

---

## 11. SCOPE GAP CHECK

**Project title claims:** "Analysis and *Prediction* of Airbnb Listing Prices"

### What the project actually delivers:

| Component | Delivered? | Notes |
|-----------|-----------|-------|
| Data import and cleaning | YES | Solid — NA handling, address standardization, column pruning |
| Exploratory Data Analysis | YES | Comprehensive — 12+ distinct visualizations, correlogram, geographic maps, treemap |
| Feature engineering | PARTIAL | EDA done on all features; categorical features explored but not encoded for modeling |
| Train/test split | YES (but incomplete) | Test set created and discarded; only validation RMSE reported |
| Baseline model | YES | Median baseline, RMSE=99.05 |
| Linear regression model | YES | RMSE=73.80; but no model summary (R², p-values absent) |
| RandomForest model | **NO** | Library loaded, model never fit |
| Decision tree model | **NO** | Library loaded, model never fit |
| Model assumption diagnostics | **NO** | No residual plots, VIF, normality tests |
| Final test set evaluation | **NO** | `airbnb_test` created but never used |
| Model interpretation | **NO** | Coefficients never printed; feature importance never computed |

### Verdict

**The project stops short of its stated goal.** It delivers solid EDA and one basic linear model with a single RMSE metric. The notebook structure implies RandomForest and rpart models were planned (hence the `library()` calls) but were never implemented. The "prediction" deliverable as-is would be rated **incomplete** for a portfolio piece — a reviewer would expect at least:

1. `summary(lm_airbnb)` output showing R², Adjusted R², and coefficient significance
2. Residual plots to validate linear model assumptions
3. At least one non-linear model (RandomForest is already imported — just needs to be called)
4. Final evaluation on the held-out `airbnb_test` set
5. Some discussion of which features are most predictive (feature importance or standardized coefficients)

The EDA section is genuinely strong (well-labeled, diverse chart types, clear narrative). The modeling section is the weak half of the project.

---

## Summary of Critical Issues (Priority Order)

| # | Severity | Issue |
|---|----------|-------|
| 1 | CRITICAL | Stadia Maps API key hardcoded in Cell 1 — revoke and rotate immediately |
| 2 | HIGH | Local folder is not a git repo — no commit history, no connection to GitHub remote |
| 3 | HIGH | Dataset name mismatch: notebook loads `Airbnb.csv`, repo has `dataset/seattle_01.csv.zip` — notebook is not reproducible from the repo |
| 4 | HIGH | `airbnb_test` held-out set is created but never evaluated — stated "prediction" goal unmet |
| 5 | HIGH | `summary(lm_airbnb)` never called — R², p-values, F-stat absent from the only model fit |
| 6 | MEDIUM | `randomForest` and `rpart` imported but never used — promised modeling scope not delivered |
| 7 | MEDIUM | No model assumption diagnostics (residuals, VIF, normality) |
| 8 | MEDIUM | No `renv.lock` or dependency manifest — not reproducible on another machine |
| 9 | MEDIUM | No LICENSE or `.gitignore` |
| 10 | MEDIUM | README is 2 sentences — no setup, no results, no dataset attribution |
| 11 | LOW | `sample.kind = "Rounding"` deprecation warning in `set.seed` |
| 12 | LOW | `size` aesthetic in ggplot2 geoms replaced by `linewidth` in ggplot2 ≥ 3.4 — will generate warnings |
| 13 | LOW | Orphaned AI self-dialogue markdown cell should be removed |
| 14 | LOW | `library(tidyverse)` is redundant — individual component packages already loaded above |
| 15 | INFO | Folder name `Script or code/` contains spaces — inconvenient for CLI; consider renaming to `scripts/` or `code/` |
