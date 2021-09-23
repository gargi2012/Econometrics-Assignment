## Exercise Questions

1. Use the help function to explore what the series gafa_stock, PBS, vic_elec and pelt represent.

 -  a. Use autoplot() to plot some of the series in these data sets.
 -  b. What is the time interval of each series?
 
2. Use filter() to find what days corresponded to the peak closing price for each of the four stocks in gafa_stock.

3. Download the file tute1.csv from the book website, open it in Excel (or some other spreadsheet application), and review its contents. You should find four columns of information. Columns B through D each contain a quarterly series, labelled Sales, AdBudget and GDP. Sales contains the quarterly sales for a small company over the period 1981-2005. AdBudget is the advertising budget and GDP is the gross domestic product. All series have been adjusted for inflation.

 - a. You can read the data into R with the following script:

tute1 <- readr::read_csv("tute1.csv")
View(tute1)
 - b. Convert the data to time series

mytimeseries <- tute1 %>%
  mutate(Quarter = yearmonth(Quarter)) %>%
  as_tsibble(index = Quarter)
- c. Construct time series plots of each of the three series

mytimeseries %>%
  pivot_longer(-Quarter) %>%
  ggplot(aes(x = Quarter, y = value, colour = name)) +
  geom_line() +
  facet_grid(name ~ ., scales = "free_y")
Check what happens when you don’t include facet_grid().

4. The USgas package contains data on the demand for natural gas in the US.

Install the USgas package.
Create a tsibble from us_total with year as the index and state as the key.
Plot the annual natural gas consumption by state for the New England area (comprising the states of Maine, Vermont, New Hampshire, Massachusetts, Connecticut and Rhode Island).
Download tourism.xlsx from the book website and read it into R using readxl::read_excel().
Create a tsibble which is identical to the tourism tsibble from the tsibble package.
Find what combination of Region and Purpose had the maximum number of overnight trips on average.
Create a new tsibble which combines the Purposes and Regions, and just has total trips by State.
Create time plots of the following four time series: Bricks from aus_production, Lynx from pelt, Close from gafa_stock, Demand from vic_elec.

Use ? (or help()) to find out about the data in each series.
For the last plot, modify the axis labels and title.
The aus_arrivals data set comprises quarterly international arrivals to Australia from Japan, New Zealand, UK and the US.

Use autoplot(), gg_season() and gg_subseries() to compare the differences between the arrivals from these four countries.
Can you identify any unusual observations?
Monthly Australian retail data is provided in aus_retail. Select one of the time series as follows (but choose your own seed value):

set.seed(12345678)
myseries <- aus_retail %>%
  filter(`Series ID` == sample(aus_retail$`Series ID`,1))
Explore your chosen retail time series using the following functions:

autoplot(), gg_season(), gg_subseries(), gg_lag(),

ACF() %>% autoplot()

Can you spot any seasonality, cyclicity and trend? What do you learn about the series?

Use the following graphics functions: autoplot(), gg_season(), gg_subseries(), gg_lag(), ACF() and explore features from the following time series: “Total Private” Employed from us_employment, Bricks from aus_production, Hare from pelt, “H02” Cost from PBS, and us_gasoline.

Can you spot any seasonality, cyclicity and trend?
What do you learn about the series?
What can you say about the seasonal patterns?
Can you identify any unusual years?
The following time plots and ACF plots correspond to four different time series. Your task is to match each time plot in the first row with one of the ACF plots in the second row.



The aus_livestock data contains the monthly total number of pigs slaughtered in Victoria, Australia, from Jul 1972 to Dec 2018. Use filter() to extract pig slaughters in Victoria between 1990 and 1995. Use autoplot() and ACF() for this data. How do they differ from white noise? If a longer period of data is used, what difference does it make to the ACF?

Use the following code to compute the daily changes in Google closing stock prices.

dgoog <- gafa_stock %>%
  filter(Symbol == "GOOG", year(Date) >= 2018) %>%
  mutate(trading_day = row_number()) %>%
  update_tsibble(index = trading_day, regular = TRUE) %>%
  mutate(diff = difference(Close))
Why was it necessary to re-index the tsibble?

Plot these differences and their ACF.

Do the changes in the stock prices look like white noise?
