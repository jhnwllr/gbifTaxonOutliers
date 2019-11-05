

get_outlier_data = function(d) {

  datasetkey = d %>%
    pull(datasetkey) %>%
    unique()

  if(unique(d$n_rows) >= 10e3) { # reduce resolution for large datasets

    d = d %>%
      select(
        Kingdom = kingdomkey,
        Phylum = phylumkey,
        Order = orderkey,
        Class = classkey,
        Family = familykey
      ) %>%
      unique() %>%
      mutate(Species = make.unique(as.character(Family))) # make "species column"

    print("over 10k")

  } else { # run full resolution for larger datasets
    d = d %>%
      select(
        Kingdom = kingdomkey,
        Phylum = phylumkey,
        Order = orderkey,
        Class = classkey,
        Family = familykey,
        Genus = genuskey
      ) %>%
      mutate(Species = make.unique(as.character(Genus)))

  }

  outlier_data = d %>%
    tibble::column_to_rownames("Species") %>%
    vegan::taxa2dist(varstep=TRUE,check=FALSE) %>%
    as.matrix() %>%
    tibble::as_tibble() %>%
    reshape2::melt() %>%
    select(variable,value) %>%
    group_by(variable) %>%
    summarise(row_dist = mean(value)) %>%
    arrange(-row_dist) %>%
    mutate(sd_row_dist = sd(row_dist)) %>%
    mutate(mean_row_dist = mean(row_dist)) %>%
    mutate(outlier = abs(row_dist - mean_row_dist)/sd_row_dist) %>%
    mutate(datasetkey = datasetkey)

  return(outlier_data)
}






