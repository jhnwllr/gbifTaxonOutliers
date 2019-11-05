

outlier_data_for_dataset_taxonkey_counts = function(file) {

  out = data.table::fread(file) %>%
    na.omit() %>%
    filter(!datasetkey == "") %>%
    group_by(datasetkey) %>%
    mutate(n_rows = n()) %>%
    filter(n_rows > 2) %>% # only run on datasets with more than 2 taxa
    ungroup() %>%
    glimpse() %>%
    group_split(datasetkey) %>%
    map(~ gbifTaxonOutliers::get_outlier_data(.x)) %>%
    bind_rows() %>%
    glimpse()

  return(out)

}
