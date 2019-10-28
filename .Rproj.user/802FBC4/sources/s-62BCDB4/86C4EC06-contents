#Compile sheffield map data with non-UK
lsoas <- st_read('F:/Data/MapPolygons/England/2011/England_lsoa_2011_clipped/england_lsoa_2011_clipped.shp')

#Oh goddam it'll all be London local authority names...
shef <- lsoas %>% 
  filter(grepl(pattern = 'Sheffield',x = .$name))

cob <- read_csv('C:/Users/Dan Olner/Dropbox/SheffieldMethodsInstitute/3D_printing2/data/countryOfBirth_Y&H_LSOA.csv')

#Tick
table(shef$code %in% cob$LSOA11CD)

#Subset to Sheffield
cob <- cob[cob$LSOA11CD %in% shef$code,]

#tidy to make easier to read
names(cob) <- gsub('; measures: Value','', names(cob))

#non-white is just 'all categories' minus UK (include all UK)
cob$nonUK <- cob$`Country of Birth: All categories: Country of birth` - 
  (cob$`Country of Birth: Europe: United Kingdom: Total` +
     cob$`Country of Birth: Europe: Great Britain not otherwise specified` +
     cob$`Country of Birth: Europe: United Kingdom not otherwise specified`)

#as % of zone pop
cob$nonUKprop <- (cob$nonUK/cob$`Country of Birth: All categories: Country of birth`)*100
hist(cob$nonUKprop)

#merge with geography - keep only the column we want for now.
#cob_geo <- merge(lsoas[,c('code')], cob[,c('LSOA11CD','nonUKZoneProp')], by.x = 'code', by.y = 'LSOA11CD')

#Add to shef.sp
shef <- merge(shef, cob[,c('LSOA11CD','nonUKprop')], by.x = 'code', by.y = 'LSOA11CD')

#Tick
plot(shef[,'nonUKprop'])

saveRDS(shef,'data/mapdata/sheffieldLSOAs_w_nonUKbornProportion.rds')
st_write(shef,'data/mapdata/sheffieldLSOAs_w_nonUKbornProportion.shp')
