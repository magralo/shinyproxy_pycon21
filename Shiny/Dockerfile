FROM  magralo95/dacshiny
## R with tidyverse, plotly, DT, Janitor, shiny, rmarkdown, leaflet, ggthemes, lubridate, jsonlite, httr and factoextra already installed


RUN R -e "install.packages(c('ggdark','leaflet.extras','shinyalert'), repos='https://cloud.r-project.org/')"

# copy the app to the image
RUN mkdir /root/pokedex
COPY pokedex/ /root/pokedex


COPY Rprofile.site /usr/lib/R/etc/

EXPOSE 3838