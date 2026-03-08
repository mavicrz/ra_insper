# RA INSPER - Núcleo Saúde -----------------------------------------------------
# Autor: Maria Cruz
# Data: 06/03/2026
# Descrição: 

# 0. Configuração --------------------------------------------------------------

# Limpar memória
rm(list=ls())
gc()

# Bibliotecas
# install.packages('xfun')
xfun::pkg_attach(c('tidyverse', 'readr'), install=T)

# 1. Coletar Dados ---------------------------------------------------------------------

# Download dos dados a partir do site FTP
purrr::map(.x = stringr::str_pad(3:4,width = 2, side = 'left', pad = 0),
           .f = ~download.file(url = stringr::str_glue('ftp://ftp.datasus.gov.br/dissemin/publicos/SIHSUS/200801_/Dados/RDSP24{.x}.dbc'),
                               method = 'curl', 
                               destfile = stringr::str_glue('data/raw/RD24{.x}.dbc')))


# 2. Limpeza dos Dados ---------------------------------------------------------------------

list_path <- list.files(path = 'data/raw/', full.names = T, pattern = '.dbc')

data <- purrr::map(.x = list_path, 
                   .f = ~read.dbc::read.dbc(file = .x)) %>%
  purrr::reduce(bind_rows)

intern_neonatais <- data %>% 
  janitor::clean_names() %>% 
  dplyr::filter(munic_mov == '355030',
                idade %in% 0:27,
                cod_idade == 2) %>% 
  dplyr::select(espec, n_aih, ident, sexo, proc_rea,
                val_tot, diag_princ, morte, cid_morte,
                raca_cor, etnia)

writexl::write_xlsx(x = intern_neonatais, path = 'data/clean/intern_neonatais.xlsx')


intern_parto <- data %>% 
  janitor::clean_names() %>% 
  dplyr::filter(munic_mov == '355030',
                cod_idade == 4,
                stringr::str_detect(diag_princ, 'O80|O81|O82|O83|O84')==T) %>% 
  dplyr::select(espec, n_aih, ident, sexo, idade, proc_rea,
                val_tot, diag_princ, morte, cid_morte,
                raca_cor, etnia, num_filhos, instru, contracep1, gestrisco, cbor)

writexl::write_xlsx(x = intern_parto, path = 'data/clean/intern_parto.xlsx')





