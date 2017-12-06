## Welcome to Kechen Hu's final project for STAT184

This webpage is about my Final project of STAT 184 class, I use the data I found from [Data.gov](https://catalog.data.gov/dataset/maryland-statewide-vehicle-crash-data-dictionary). The data is about car crash in Maryland state.

### Markdown
I will provide my code in this section, but I will link the html at the bottom so you will have a better idea about my project.
```markdown
library(ggplot2)
library(lubridate)
Crash <- read.csv("Crash_Reporting_-_Drivers_Data.csv")
Vehicle_type<-
  Crash %>% 
    mutate(date=as.Date(mdy_hms(Crash.Date.Time))) %>% 
    select(date,Vehicle.Body.Type)
Car<-
  Vehicle_type %>% 
  group_by(Vehicle.Body.Type) %>% 
  summarize(total=n())
head(arrange(Car,desc(total)),n=3)
Vehicle_type %>% 
  group_by(date,Vehicle.Body.Type) %>% 
  summarize(count=n()) %>% 
  ggplot(aes(x=date,y=count)) + geom_point(size=0.5) + facet_wrap( ~ Vehicle.Body.Type)
Vehicle_type %>% 
  group_by(date,Vehicle.Body.Type) %>% 
  summarize(count=n()) %>% 
  ggplot(aes(x=date,y=count)) + geom_point(size=0.1) + facet_wrap( ~ Vehicle.Body.Type) + ylim(0,20)
library(maps)
C_map <- map_data("state","Maryland")
counties <- map_data("county")
C_county <- subset(counties, region == "maryland")
map1 <- ggplot(C_county) +
  geom_polygon(aes(x = long, y = lat, group = group), fill = "gray", colour = "white")
map1 + geom_point(data = Crash, aes(x = Longitude, y = Latitude), colour = "red",size=0.01)
Passenger <-
  Crash %>% 
  mutate(date=as.Date(mdy_hms(Crash.Date.Time))) %>% 
  select(Vehicle.Body.Type,Weather, Light, date) %>% 
  filter(Vehicle.Body.Type== "PASSENGER CAR")
keeps <- levels(Passenger$Weather)
keeps <- keeps[-c(6, 7, 12)]
Passenger %>% 
  filter(Weather %in% keeps) %>% 
  filter(Light %in% c("DARK NO LIGHTS","DAWN","DAYLIGHT","DUSK")) %>% 
  group_by(date,Vehicle.Body.Type, Weather,Light) %>% 
  summarize(count=n()) %>% 
  ggplot(aes(x=date,y=count,color=Weather,shape=Light)) + geom_point(size=1)
```

For the detailed information, please [check](https://cdn.rawgit.com/Grayhu07/Final_Project_STAT184/43bc816e/final_project.html)

