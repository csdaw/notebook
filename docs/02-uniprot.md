

```r
renv::activate(profile = "02-uniprot")
```

# UniProt


## Querying UniProt programmatically using R

### Retrieve/ID mapping

For a given list of UniProt accessions, obtain some metadata. The output you 
use must be `tab` or ...

The equivalent action in the UniProt website would be to:

- Go to [Retrieve/ID mapping](https://www.uniprot.org/uploadlists/)
- Paste your IDs
- Click 'Submit'
- Click 'Columns'
- Choose the columns of interest by name

See [this page](https://www.uniprot.org/help/uniprotkb_column_names) for a list
of column names available.


```r
# some random proteins of interest
my_proteins <- c("A5YKK6", "P02786")

# the URL we will be querying
my_url <- "https://www.uniprot.org/uploadlists/"
```

#### Using `httr2` package


```r
library(httr2)
```


```r
# construct request
req <- request(my_url) %>% 
  req_url_query(
    `query` = paste(my_proteins, collapse = " "),
    `from` = "ACC+ID", 
    `to` = "ACC",
    `format` = "tab", 
    `columns` = paste("id", "features", "go-id", sep = ",")
  )
```


```r
# look at request before we actually send it
req %>% req_dry_run()
#> GET /uploadlists/?query=A5YKK6%20P02786&from=ACC%2BID&to=ACC&format=tab&columns=id%2Cfeatures%2Cgo-id HTTP/1.1
#> Host: www.uniprot.org
#> User-Agent: httr2/0.1.1 r-curl/4.3.2 libcurl/7.64.1
#> Accept: */*
#> Accept-Encoding: deflate, gzip
```


```r
# send request and obtain response
resp <- req %>% req_perform()
resp
#> <httr2_response>
#> GET
#> https://www.uniprot.org/uniprot/?query=yourlist:M2022040592C7BAECDB1C5C413EE0E0348724B6824948FAE&sort=yourlist:M2022040592C7BAECDB1C5C413EE0E0348724B6824948FAE&format=tab&columns=id,features,go-id,yourlist(M2022040592C7BAECDB1C5C413EE0E0348724B6824948FAE)
#> Status: 200 OK
#> Content-Type: text/plain
#> Body: In memory (1787 bytes)
```


```r
# parse response
my_table <- resp %>% 
  resp_body_string() %>% 
  read.table(text = ., sep = "\t", header = TRUE)
```


```r
head(my_table)
#>    Entry
#> 1 A5YKK6
#> 2 P02786
#>                                                                                                                                                                                                                                                                                                          Features
#> 1                                                       Alternative sequence (5); Beta strand (10); Chain (1); Compositional bias (2); Erroneous initiation (1); Frameshift (1); Helix (75); Modified residue (2); Motif (7); Mutagenesis (6); Natural variant (19); Region (4); Sequence conflict (9); Turn (10)
#> 2 Beta strand (27); Chain (2); Disulfide bond (2); Domain (1); Erroneous initiation (1); Glycosylation (4); Helix (28); Lipidation (2); Modified residue (5); Motif (3); Mutagenesis (25); Natural variant (5); Region (2); Sequence conflict (3); Site (1); Topological domain (2); Transmembrane (1); Turn (10)
#>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        Gene.ontology.IDs
#> 1                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     GO:0000122; GO:0000288; GO:0000932; GO:0001829; GO:0003723; GO:0005615; GO:0005634; GO:0005778; GO:0005829; GO:0010606; GO:0016020; GO:0017148; GO:0019904; GO:0030014; GO:0030015; GO:0030331; GO:0033147; GO:0035195; GO:0042974; GO:0048387; GO:0060090; GO:0060213; GO:0061014; GO:0070016; GO:0090503; GO:1900153; GO:2000036
#> 2 GO:0001618; GO:0001666; GO:0001934; GO:0003723; GO:0003725; GO:0004998; GO:0005576; GO:0005615; GO:0005634; GO:0005739; GO:0005768; GO:0005769; GO:0005886; GO:0005887; GO:0005905; GO:0006826; GO:0006879; GO:0006953; GO:0007568; GO:0007584; GO:0008235; GO:0009897; GO:0009986; GO:0010008; GO:0010039; GO:0010042; GO:0010628; GO:0010637; GO:0016020; GO:0016323; GO:0019901; GO:0030316; GO:0030544; GO:0030669; GO:0030890; GO:0031334; GO:0031410; GO:0031623; GO:0032526; GO:0033138; GO:0033572; GO:0035556; GO:0042102; GO:0042470; GO:0042802; GO:0042803; GO:0043066; GO:0043123; GO:0043231; GO:0044877; GO:0045780; GO:0045830; GO:0046688; GO:0048471; GO:0051087; GO:0051092; GO:0055037; GO:0055038; GO:0070062; GO:0071466; GO:0072562; GO:0150104; GO:1900182; GO:1903561; GO:1990712; GO:1990830
#>   yourlist.M2022040592C7BAECDB1C5C413EE0E0348724B6824948FAE
#> 1                                                    A5YKK6
#> 2                                                    P02786
```


#### Using `httr` package


```r
library(httr)
```


```r
# construct request
req <- list(
  query = paste(my_proteins, collapse = " "),
  from = "ACC+ID",
  to = "ACC",
  format = "tab",
  columns = paste("id", "features", "go-id", sep = ",")
)
```


```r
# look at request before we actually send it
httr::modify_url(my_url, query = req)
#> [1] "https://www.uniprot.org/uploadlists/?query=A5YKK6%20P02786&from=ACC%2BID&to=ACC&format=tab&columns=id%2Cfeatures%2Cgo-id"
```


```r
# send request and obtain response
resp <- httr::GET(
  url = my_url,
  query = req
)
resp$request
#> <request>
#> GET https://www.uniprot.org/uploadlists/?query=A5YKK6%20P02786&from=ACC%2BID&to=ACC&format=tab&columns=id%2Cfeatures%2Cgo-id
#> Output: write_memory
#> Options:
#> * useragent: libcurl/7.64.1 r-curl/4.3.2 httr/1.4.2
#> * httpget: TRUE
#> Headers:
#> * Accept: application/json, text/xml, application/xml, */*
```


```r
# parse response
my_table <- resp %>% 
  httr::content(type = "text", encoding = "UTF8") %>% 
  read.table(text = ., sep = "\t", header = TRUE)
```


```r
head(my_table)
#>    Entry
#> 1 A5YKK6
#> 2 P02786
#>                                                                                                                                                                                                                                                                                                          Features
#> 1                                                       Alternative sequence (5); Beta strand (10); Chain (1); Compositional bias (2); Erroneous initiation (1); Frameshift (1); Helix (75); Modified residue (2); Motif (7); Mutagenesis (6); Natural variant (19); Region (4); Sequence conflict (9); Turn (10)
#> 2 Beta strand (27); Chain (2); Disulfide bond (2); Domain (1); Erroneous initiation (1); Glycosylation (4); Helix (28); Lipidation (2); Modified residue (5); Motif (3); Mutagenesis (25); Natural variant (5); Region (2); Sequence conflict (3); Site (1); Topological domain (2); Transmembrane (1); Turn (10)
#>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        Gene.ontology.IDs
#> 1                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     GO:0000122; GO:0000288; GO:0000932; GO:0001829; GO:0003723; GO:0005615; GO:0005634; GO:0005778; GO:0005829; GO:0010606; GO:0016020; GO:0017148; GO:0019904; GO:0030014; GO:0030015; GO:0030331; GO:0033147; GO:0035195; GO:0042974; GO:0048387; GO:0060090; GO:0060213; GO:0061014; GO:0070016; GO:0090503; GO:1900153; GO:2000036
#> 2 GO:0001618; GO:0001666; GO:0001934; GO:0003723; GO:0003725; GO:0004998; GO:0005576; GO:0005615; GO:0005634; GO:0005739; GO:0005768; GO:0005769; GO:0005886; GO:0005887; GO:0005905; GO:0006826; GO:0006879; GO:0006953; GO:0007568; GO:0007584; GO:0008235; GO:0009897; GO:0009986; GO:0010008; GO:0010039; GO:0010042; GO:0010628; GO:0010637; GO:0016020; GO:0016323; GO:0019901; GO:0030316; GO:0030544; GO:0030669; GO:0030890; GO:0031334; GO:0031410; GO:0031623; GO:0032526; GO:0033138; GO:0033572; GO:0035556; GO:0042102; GO:0042470; GO:0042802; GO:0042803; GO:0043066; GO:0043123; GO:0043231; GO:0044877; GO:0045780; GO:0045830; GO:0046688; GO:0048471; GO:0051087; GO:0051092; GO:0055037; GO:0055038; GO:0070062; GO:0071466; GO:0072562; GO:0150104; GO:1900182; GO:1903561; GO:1990712; GO:1990830
#>   yourlist.M20220405F248CABF64506F29A91F8037F07B67D149501B3
#> 1                                                    A5YKK6
#> 2                                                    P02786
```

