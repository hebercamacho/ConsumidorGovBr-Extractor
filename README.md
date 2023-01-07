# ConsumidorGovBr-Extractor
Extrator de reclamações do site consumidor.gov.br via Web Scraping (raspa tela)

>Autoria: Héber Camacho Desterro

- [ConsumidorGovBr-Extractor](#consumidorgovbr-extractor)
  - [Objetivo](#objetivo)
  - [Como utilizar](#como-utilizar)
    - [Parâmetros](#parâmetros)
      - [palavrasChave (list of str)](#palavraschave-list-of-str)
      - [numero\_maximo\_resultados (int)](#numero_maximo_resultados-int)
      - [segmentoMercado (int or str)](#segmentomercado-int-or-str)
      - [fornecedor (str)](#fornecedor-str)
      - [regiao (int or str)](#regiao-int-or-str)
      - [uf (int or str)](#uf-int-or-str)
      - [cidade (int or str)](#cidade-int-or-str)
      - [area (str)](#area-str)
      - [assunto (str)](#assunto-str)
      - [problema (str)](#problema-str)
      - [dataInicio (str)](#datainicio-str)
      - [dataTermino (str)](#datatermino-str)
      - [avaliacao (int or str)](#avaliacao-int-or-str)
      - [nota (int or str)](#nota-int-or-str)
  - [Performance](#performance)


## Objetivo
Extrair dados estruturados das reclamações do site consumidor.gov.br, em formato .csv e .xlsx, com diversos filtros disponíveis.

## Como utilizar
1. Abra o arquivo ExtrairConsumidorGovBr.ipynb utilizando o Google Colab, Jupyter Notebook ou IDE de sua preferência
2. Na célula ``Definição dos parâmetros de busca``, defina os filtros que quer utilizar, como a lista de palavras-chave. Veja as opções na seção [Parâmetros](#parâmetros).
3. Execute todas as células em ordem, aguarde a execução do algoritmo de busca. Um arquivo .xlsx e um arquivo .csv com o resultado serão salvos automaticamente no diretório de execução .
> O nome dos arquivos será no formato: ReclamacoesConsumidor-{Numero de Resultados}-{Lista de Palavras-chave}. 
> 
> Exemplo: ReclamacoesConsumidor-10-atendimento_reclamacao.csv

### Parâmetros
Cada parâmetro passado no início da execução será utilizado como um filtro da busca. Veja a seguir como utilizar cada um deles.

#### palavrasChave (list of str)
Uma lista de strings contendo cada palavra que será utilizada na busca por resultados.  
As palavras são buscadas nos campos Relato, Resposta e Avaliação.  
Recomenda-se colocar cada palavra separadamente em uma string, pois o algoritmo é executado mais rapidamente e não há diferença no resultado final para palavras colocadas juntas (exceto pela ordenação).  

Exemplo:  
```python
# consulta reclamações que contenham qualquer uma das palavras: atendimento ou telefone
palavrasChave=['atendimento', 'telefone']
```
Para forçar a busca conjunta de duas palavras, coloque as duas na mesma string entre aspas duplas. **Isso não significa que as duas palavras aparecerão justapostas nos resultados, apenas que as duas aparecerão em alguma posição**.  

Exemplo:  
```python
# consulta reclamações que contenham as palavras: atendimento E telefone (não necessariamente juntas)
palavrasChave=['"atendimento telefone"']
```
A busca é *case-insensitive* e desconsidera acentuação nas palavras.  

Exemplo:  
```python
palavrasChave=['Telefônico']
# fornece o mesmo resultado que:
palavrasChave=['telefonico']
```
Para não utilizar esse filtro, mantenha da seguinte maneira:
```python
palavrasChave=['']
```

#### numero_maximo_resultados (int)
O número máximo de resultados que serão retornados na base. None ou 0 retornarão todos os resultados encontrados.  
**Importante: precisa ser um número inteiro positivo múltiplo de 10. Ex: 0, 10, 20, 1000...**

Exemplo:  
```python
numero_maximo_resultados=None  # Não limitar resultados
numero_maximo_resultados=0     # Não limitar resultados
numero_maximo_resultados=5     # Gera Exception, precisa ser múltiplo de 10
numero_maximo_resultados=100   # Limitar a 100 resultados
```

#### segmentoMercado (int or str)
O ID do segmento de mercado das empresas.  

Exemplo:  
```python
segmentoMercado=''  # Não filtrar por segmento
segmentoMercado=5  # 5 = Bancos, Financeiras e Administradoras de Cartão
```

Lista de IDs atual:

| ID   | Nome do segmento                                                        |
| :--- | :---------------------------------------------------------------------- |
| 7    | Administradoras de Consórcios                                           |
| 210  | Agua e Saneamento                                                       |
| 251  | Aluguel de Carros                                                       |
| 90   | Bancos de Dados e Cadastros de Consumidores                             |
| 5    | Bancos, Financeiras e Administradoras de Cartão                         |
| 130  | Bares, Restaurantes, Casas Noturnas e Similares                         |
| 110  | Cartões de Descontos                                                    |
| 9    | Comércio Eletrônico                                                     |
| 21   | Construtoras, Incorporadoras e Imobiliárias                             |
| 271  | Corretoras e Distribuidoras de Títulos e Investimentos                  |
| 172  | Distribuidoras de Combustíveis / Gás                                    |
| 91   | Editoras e Veículos de Imprensa                                         |
| 173  | Empresas de Intermediação de Serviços / Negócios                        |
| 10   | Empresas de Pagamento Eletrônico                                        |
| 16   | Empresas de Recuperação de Crédito                                      |
| 25   | Empresas de Serviços Postais e Logística                                |
| 14   | Energia Elétrica                                                        |
| 190  | Entidades Sem Fins Lucrativos                                           |
| 231  | Entretenimento                                                          |
| 22   | Estabelecimentos de Ensino                                              |
| 13   | Fabricantes - Eletroeletrônicos, Produtos de Telefonia e Informática    |
| 20   | Fabricantes - Eletroportáteis e Artigos de Uso Doméstico e Pessoal      |
| 12   | Fabricantes - Linha Branca                                              |
| 23   | Fabricantes - Móveis, Colchões e Acessórios                             |
| 27   | Fabricantes - Produtos Alimentícios                                     |
| 170  | Fabricantes - Produtos Químicos e Farmacêuticos                         |
| 17   | Farmácias                                                               |
| 28   | Hospitais, Clínicas, Laboratórios e Outros Serviços de Saúde            |
| 70   | Material de Construção, Acabamento e Ferramentas                        |
| 29   | Montadoras, Concessionárias e Prestadores de Serviços Automotivos       |
| 2    | Operadoras de Planos de Saúde e Administradoras de Benefícios           |
| 1    | Operadoras de Telecomunicações (Telefonia, Internet, TV por assinatura) |
| 18   | Perfumaria, Cosméticos e Higiene Pessoal                                |
| 50   | Programas de Fidelidade                                                 |
| 30   | Provedores de Conteúdo e Outros Serviços na Internet                    |
| 6    | Seguros, Capitalização e Previdência                                    |
| 24   | Serviços Esportivos                                                     |
| 291  | Shopping Centers                                                        |
| 3    | Supermercados                                                           |
| 4    | Transporte Aéreo                                                        |
| 26   | Transporte Terrestre                                                    |
| 8    | Varejo                                                                  |
| 19   | Vestuário, Calçados e Acessórios                                        |
| 15   | Viagens, Turismo e Hospedagem                                           |

#### fornecedor (str)
Nome da empresa. 
**Importante o nome ser exatamente igual ao da lista a seguir.**

Exemplo:  
```python
fornecedor=''  # Não filtrar por empresa
fornecedor='Black & Decker'   # Filtrar por esta empresa. Copiar e colar para não errar os caracteres especiais
```

Lista de empresas atual:
| Nome da empresa                                                                                   |
| :------------------------------------------------------------------------------------------------ |
| 123 Milhas                                                                                        |
| 3corações                                                                                         |
| 3tentos                                                                                           |
| 88i Seguradora Digital                                                                            |
| 99App                                                                                             |
| 99Food                                                                                            |
| 99Pay                                                                                             |
| Abastece Aí - Km de Vantagens                                                                     |
| Acentra                                                                                           |
| Acredicoop                                                                                        |
| Ademicon Administradora de Consórcios (antigas Ademilar e Conseg)                                 |
| Adidas                                                                                            |
| Administradora de Consórcios Sicredi                                                              |
| Aerolíneas Argentinas                                                                             |
| Aeromexico                                                                                        |
| Afinz (Sorocred)                                                                                  |
| Agaxtur Viagens                                                                                   |
| Age Telecom                                                                                       |
| Agemed Planos de Saúde                                                                            |
| Agibank Financeira                                                                                |
| Ágil                                                                                              |
| Agillitas                                                                                         |
| Ágora Investimentos                                                                               |
| Águas Alta Floresta                                                                               |
| Águas Andradina                                                                                   |
| Águas Canarana                                                                                    |
| Águas Castilho                                                                                    |
| Águas Colíder                                                                                     |
| Águas Comodoro                                                                                    |
| Águas Cuiabá                                                                                      |
| Águas da Condessa                                                                                 |
| Águas das Agulhas Negras                                                                          |
| Águas de Araçoiaba                                                                                |
| Águas de Jahu                                                                                     |
| Águas de Juturnaíba                                                                               |
| Águas de Manaus                                                                                   |
| Águas de Niterói                                                                                  |
| Águas de Nova Friburgo                                                                            |
| Águas de Pará de Minas                                                                            |
| Águas de Paraty                                                                                   |
| Águas de Timon                                                                                    |
| Águas de Votorantim                                                                               |
| Águas do Imperador                                                                                |
| Águas do Paraíba                                                                                  |
| Águas do Rio                                                                                      |
| Águas Guariroba                                                                                   |
| Águas Piquete                                                                                     |
| Águas Pontes e Lacerda                                                                            |
| AIG Seguros                                                                                       |
| Aigle Azur (DESATIVADA)                                                                           |
| Ailos Corretora de Seguros                                                                        |
| Air Canada                                                                                        |
| Air China                                                                                         |
| Air Europa                                                                                        |
| Air France                                                                                        |
| Airbnb                                                                                            |
| Ajinomoto do Brasil                                                                               |
| AL5 Bank                                                                                          |
| Alelo - Veloe - Pede Pronto                                                                       |
| Alfa Arrendamento Mercantil                                                                       |
| Alfa Corretora                                                                                    |
| Alfa Previdência e Vida                                                                           |
| Alfa Seguradora                                                                                   |
| Algar Celular                                                                                     |
| Algar Fixo                                                                                        |
| Algar TV (DESATIVADA)                                                                             |
| Aliança Administradora                                                                            |
| Aliança da Bahia                                                                                  |
| Aliança do Brasil (BB Seguros)                                                                    |
| Aliança Móveis                                                                                    |
| Alitalia                                                                                          |
| Allcare Administradora de Benefícios                                                              |
| Allianz Seguros                                                                                   |
| ALM Seguradora                                                                                    |
| Alpargatas (Havaianas, Dupé)                                                                      |
| Amaszonas Línea Aérea                                                                             |
| Amazon.com.br                                                                                     |
| Amazonas by Viverde                                                                               |
| Amazonas Energia                                                                                  |
| Ame Digital                                                                                       |
| American Airlines                                                                                 |
| American Express - Amex                                                                           |
| American Life Seguros                                                                             |
| Americanas Viagens                                                                                |
| Americanas.com                                                                                    |
| Americanas                                                                                        |
| Amil                                                                                              |
| Amvox                                                                                             |
| Anapi                                                                                             |
| Âncora Consórcios                                                                                 |
| Angelus Seguros                                                                                   |
| AOC                                                                                               |
| Aplicap Capitalização                                                                             |
| APLUB (DESATIVADA)                                                                                |
| Apple                                                                                             |
| Aqbank Pagamentos                                                                                 |
| Argo Seguros                                                                                      |
| Artex                                                                                             |
| Artezanal                                                                                         |
| Aruana Seguradora                                                                                 |
| ASAAS                                                                                             |
| Asabasp Brasil                                                                                    |
| Asbapi - Associação Brasileira de Aposentados, Pensionistas e Idosos                              |
| Aspecir Previdência                                                                               |
| Aspecir Sociedade de Crédito                                                                      |
| Assaí Atacadista                                                                                  |
| Associação Comercial de Sorocaba                                                                  |
| Assurant Seguradora                                                                               |
| Asus                                                                                              |
| Atacadão                                                                                          |
| Ativa Investimentos                                                                               |
| Ativos S.A                                                                                        |
| Atlântico Fundo de Investimento                                                                   |
| Atlas Eletrodomésticos                                                                            |
| Atradius Crédito y Caución                                                                        |
| Audi Center - Blumenau, Criciúma, Florianópolis, Joinville                                        |
| Austral Lineas Aereas - Cielos del Sur (DESATIVADA)                                               |
| AUX                                                                                               |
| Avianca - Oceanair (DESATIVADA)                                                                   |
| Avianca - Voos internacionais                                                                     |
| Avior Airlines                                                                                    |
| Avla Seguros                                                                                      |
| Avon                                                                                              |
| AXA Corporate Solutions Seguros                                                                   |
| Axa Seguros                                                                                       |
| Azul Cargo                                                                                        |
| Azul Linhas Aéreas                                                                                |
| Azul Seguros Cia de Seguros Gerais                                                                |
| Azul Viagens                                                                                      |
| B.blend Máquinas e Bebidas                                                                        |
| Backseg                                                                                           |
| Bahamas Cred                                                                                      |
| Baianão Móveis                                                                                    |
| Banco ABC                                                                                         |
| Banco Agibank (Agiplan)                                                                           |
| Banco Alfa de Investimento                                                                        |
| Banco Alfa                                                                                        |
| Banco Arbi                                                                                        |
| Banco Bari                                                                                        |
| Banco BMG                                                                                         |
| Banco Bradesco                                                                                    |
| Banco BS2                                                                                         |
| Banco BTG Pactual                                                                                 |
| Banco BV (antigo Banco Votorantim)                                                                |
| Banco C6 Consignado (Banco Ficsa)                                                                 |
| Banco Cetelem                                                                                     |
| Banco Cifra (DESATIVADA - atual Banco BMG)                                                        |
| Banco Crefisa                                                                                     |
| Banco da Amazônia                                                                                 |
| Banco Daycoval                                                                                    |
| Banco Digimais (antigo Banco Renner)                                                              |
| Banco Digio                                                                                       |
| Banco do Brasil                                                                                   |
| Banco do Nordeste                                                                                 |
| Banco Fiat                                                                                        |
| Banco Fibra                                                                                       |
| Banco Honda                                                                                       |
| Banco Hyundai Capital Brasil                                                                      |
| Banco Inbursa                                                                                     |
| Banco Industrial do Brasil (BIB)                                                                  |
| Banco Inter (Banco Intermedium)                                                                   |
| Banco Itaú Unibanco                                                                               |
| Banco Letsbank                                                                                    |
| Banco Master (antigo Banco Máxima)                                                                |
| Banco Mercantil do Brasil                                                                         |
| Banco Modal                                                                                       |
| Banco Neon (DESATIVADA)                                                                           |
| Banco Next                                                                                        |
| Banco Olé Consignado                                                                              |
| Banco Original                                                                                    |
| Banco Pan                                                                                         |
| Banco Paulista                                                                                    |
| Banco Pine                                                                                        |
| Banco PSA                                                                                         |
| Banco Rendimento                                                                                  |
| Banco Rodobens                                                                                    |
| Banco Safra                                                                                       |
| Banco Santander Cartões                                                                           |
| Banco Santander                                                                                   |
| Banco Semear                                                                                      |
| Banco Sofisa                                                                                      |
| Banco Topázio                                                                                     |
| Banco Toyota                                                                                      |
| Banco Voiter                                                                                      |
| Banco Volkswagen                                                                                  |
| Banco XP                                                                                          |
| Banco Yamaha                                                                                      |
| Banco24Horas (TecBan)                                                                             |
| Bancoob                                                                                           |
| BancoSeguro                                                                                       |
| Banese - Banco do Estado de Sergipe                                                               |
| BaneseCard                                                                                        |
| Banestes Seguros                                                                                  |
| Banestes                                                                                          |
| Banpará                                                                                           |
| Banrisul                                                                                          |
| Bayer - Divisão Agrícola                                                                          |
| Bayer - Divisão Animal                                                                            |
| Bayer - Divisão Humana                                                                            |
| Bayer - Saúde Ambiental                                                                           |
| BB Consórcios                                                                                     |
| BB Seguro Auto                                                                                    |
| BCV (DESATIVADA - atual Banco BMG)                                                                |
| Beautybox                                                                                         |
| Becker Financeira                                                                                 |
| BeeFitness.com.br                                                                                 |
| Beleza na Web Pro                                                                                 |
| Beleza na Web                                                                                     |
| Belfix                                                                                            |
| Bem Brasil Alimentos                                                                              |
| Bemol                                                                                             |
| Benevix                                                                                           |
| Benx Incorporadora                                                                                |
| Berkley Brasil Seguros                                                                            |
| Betânia Lácteos                                                                                   |
| Big Bompreço                                                                                      |
| BIG                                                                                               |
| Bil Intercâmbios                                                                                  |
| Bimbo do Brasil (Pullman, Plusvita, Ana Maria, Nutrella, Rap10, Crocantíssimo)                    |
| Bitz                                                                                              |
| Black &amp; Decker                                                                                |
| Blockbuster (DESATIVADA)                                                                          |
| Blockbuster online (DESATIVADA)                                                                   |
| BMG Seguros                                                                                       |
| BMP Money Plus                                                                                    |
| Boa Vista Serviços - SCPC (Serviço Central de Proteção ao Crédito)                                |
| BobStore - Loja Online                                                                            |
| Boehringer Ingelheim - Saúde Animal                                                               |
| Boehringer Ingelheim - Saúde Humana                                                               |
| Boletoflex                                                                                        |
| Boliviana de Aviación - BoA                                                                       |
| Booking.com                                                                                       |
| BR Qualy Consórcio                                                                                |
| Bradescard                                                                                        |
| Bradesco Auto/RE                                                                                  |
| Bradesco Capitalização                                                                            |
| Bradesco Cartões                                                                                  |
| Bradesco Consórcio                                                                                |
| Bradesco Financiamentos                                                                           |
| Bradesco Leasing                                                                                  |
| Bradesco Promotora (DESATIVADA)                                                                   |
| Bradesco Saúde                                                                                    |
| Bradesco Seguros                                                                                  |
| Bradesco Vida e Previdência                                                                       |
| Brasilcap                                                                                         |
| BrasilCard                                                                                        |
| Brasilprev Seguros e Previdência                                                                  |
| Brasilseg (BB Seguros)                                                                            |
| Brastemp                                                                                          |
| BRB - Banco de Brasília                                                                           |
| BRBCard                                                                                           |
| Breitkopf Caminhões                                                                               |
| Breitkopf Veículos                                                                                |
| Bretas                                                                                            |
| Brisanet Telecom                                                                                  |
| Britânia                                                                                          |
| British Airways                                                                                   |
| BRK Ambiental - Região Metropolitana de Maceió                                                    |
| BRK Ambiental Maranhão                                                                            |
| BRK Ambiental Tocantins (Saneatins)                                                               |
| BRK Financeira                                                                                    |
| BTG Pactual Seguros                                                                               |
| BTG Pactual Vida e Previdência                                                                    |
| BuscaPé                                                                                           |
| Buser Brasil                                                                                      |
| Buson (Guichê Virtual)                                                                            |
| BV Financeira (DESATIVADA)                                                                        |
| BV Leasing (DESATIVADA)                                                                           |
| C&amp;A Pay                                                                                       |
| C&amp;A                                                                                           |
| C6 Bank                                                                                           |
| Cabo Verde Airlines                                                                               |
| Caboimagem TV e Internet Caraguá (DESATIVADA)                                                     |
| Caboimagem TV e Internet Ubatuba (DESATIVADA)                                                     |
| CAC Engenharia                                                                                    |
| Cadence Eletrodomésticos                                                                          |
| Caedu                                                                                             |
| Cagece                                                                                            |
| Cagepa                                                                                            |
| Caixa Econômica Federal                                                                           |
| Caixa Residencial                                                                                 |
| Caixa Seguradora                                                                                  |
| Caixa Seguros Saúde                                                                               |
| Caixa Vida e Previdência                                                                          |
| Calçados Manuel                                                                                   |
| Camicado                                                                                          |
| Camil                                                                                             |
| Capemisa Capitalização                                                                            |
| Capemisa Seguradora                                                                               |
| Capital Consig                                                                                    |
| Caramuru Alimentos                                                                                |
| Cardif Seguros e Garantias                                                                        |
| Cardif Vida                                                                                       |
| Care Plus Medicina Assistencial                                                                   |
| Carrefour.com                                                                                     |
| Carrefour                                                                                         |
| Cartão Americanas.com (DESATIVADA - atual Banco Cetelem)                                          |
| Cartão Atacadão                                                                                   |
| Cartão Caedu (Administradora de Cartão de Crédito Palma)                                          |
| Cartão Carrefour                                                                                  |
| Cartão Cencosud Bretas                                                                            |
| Cartão Cencosud GBarbosa                                                                          |
| Cartão de Todos                                                                                   |
| Cartão Quero-Quero VerdeCard                                                                      |
| Cartão Sams                                                                                       |
| Cartão Shoptime (DESATIVADA - atual Banco Cetelem)                                                |
| Cartão Submarino (DESATIVADA - atual Banco Cetelem)                                               |
| Cartões Itaú                                                                                      |
| Cartões Renner (Realize CFI)                                                                      |
| Cartola                                                                                           |
| Casa &amp; Vídeo                                                                                  |
| Casa do Crédito                                                                                   |
| Casan - Companhia Catarinense de Águas e Saneamento                                               |
| Casas Bahia                                                                                       |
| Casasbahia.com                                                                                    |
| CCB Brasil Crédito, Financiamentos e Investimentos                                                |
| CCE (Digibrás)                                                                                    |
| CDL Porto Alegre                                                                                  |
| CEDAE                                                                                             |
| CEEE Distribuição                                                                                 |
| Célebre                                                                                           |
| Celesc                                                                                            |
| Cemig                                                                                             |
| Centauro-ON Vida e Previdência                                                                    |
| Centauro.com.br                                                                                   |
| Centauro                                                                                          |
| Centrape – Central Nacional dos Aposentados e Pensionistas do Brasil                              |
| Centro Universitário FMU                                                                          |
| Centro Universitário Fundação Santo André - FSA                                                   |
| Centro Universitário Unifatecie                                                                   |
| Ceprag                                                                                            |
| Cerbranorte                                                                                       |
| Cerej                                                                                             |
| Cerim                                                                                             |
| Cersul                                                                                            |
| Cesan                                                                                             |
| Chevrolet Serviços Financeiros (Banco GM)                                                         |
| Chevrolet                                                                                         |
| Chilli Beans (DESATIVADA)                                                                         |
| China Construction Bank (CCB Brasil)                                                              |
| Chubb Seguros                                                                                     |
| CI Central de Intercâmbio Viagens                                                                 |
| Cia de Automóveis Slaviero - Curitiba                                                             |
| Ciclic                                                                                            |
| Cielo                                                                                             |
| Cinemark                                                                                          |
| Citibank (DESATIVADA - atual Itaú Unibanco)                                                       |
| City Lar (DESATIVADA)                                                                             |
| Civia                                                                                             |
| Claro Celular                                                                                     |
| Claro Fixo - Embratel                                                                             |
| Claro TV                                                                                          |
| Clear Corretora                                                                                   |
| Clinipam                                                                                          |
| Clube de Saúde                                                                                    |
| CNF Consórcio                                                                                     |
| CNP Capitalização (Antiga Caixa Capitalização)                                                    |
| CNP Consórcio (Antiga Caixa Consórcios)                                                           |
| Cocel - Companhia Campolarguense de Energia                                                       |
| Colchões Gazin                                                                                    |
| Colégio Copérnico                                                                                 |
| Colormaq                                                                                          |
| Combate                                                                                           |
| Comgás - Companhia de Gás de São Paulo                                                            |
| Compaq                                                                                            |
| Compesa                                                                                           |
| Compra Certa                                                                                      |
| Comprei Pontuei (DESATIVADA)                                                                      |
| Comprev Seguradora                                                                                |
| Comprev Sociedade de Crédito Direto - COMPREVFIN                                                  |
| Comprev Vida e Previdência                                                                        |
| Concessionária CCR MSVia                                                                          |
| Concessionária CCR RioSP                                                                          |
| Concessionária CCR ViaCosteira                                                                    |
| Concessionária CCR ViaSul                                                                         |
| Concessionária Concer                                                                             |
| Concessionária Eco050                                                                             |
| Concessionária Eco101                                                                             |
| Concessionária Ecoponte                                                                           |
| Concessionária EcoRioMinas                                                                        |
| Concessionária Ecosul                                                                             |
| Concessionária Ecovias do Araguaia                                                                |
| Concessionária Ecovias do Cerrado                                                                 |
| Concessionária Rio Teresópolis CRT                                                                |
| Concessionária Rodovia Arteris Fluminense                                                         |
| Concessionária Rodovia Arteris Litoral Sul                                                        |
| Concessionária Rodovia do Aço                                                                     |
| Concessionária Rodovia Fernão Dias                                                                |
| Concessionária Rodovia Planalto Sul                                                               |
| Concessionária Rodovia Régis Bittencourt                                                          |
| Concessionária Rota do Oeste                                                                      |
| Concessionária Triunfo Concebra                                                                   |
| Concessionária Triunfo Transbrasiliana                                                            |
| Concessionária Via 040                                                                            |
| Concessionária ViaBahia                                                                           |
| Condor                                                                                            |
| ConectCar                                                                                         |
| Conseg Consórcios (DESATIVADA)                                                                    |
| Consórcio Breitkopf                                                                               |
| Consórcio Embracon                                                                                |
| Consórcio Fiat                                                                                    |
| Consórcio Gazin                                                                                   |
| Consórcio Luiza                                                                                   |
| Consórcio Mercedes Benz                                                                           |
| Consórcio Nacional Chevrolet                                                                      |
| Consórcio Nacional Govesa                                                                         |
| Consórcio Nacional Honda                                                                          |
| Consórcio Nacional Volkswagen                                                                     |
| Consórcio Primo Rossi ABC                                                                         |
| Consórcio Realiza (DESATIVADA)                                                                    |
| Consórcio Remaza                                                                                  |
| Consórcio Reserva                                                                                 |
| Consórcio Unilance (DESATIVADA)                                                                   |
| Consórcio Zema                                                                                    |
| Consul                                                                                            |
| Contabilista Papelaria e Informática                                                              |
| Continental Center                                                                                |
| Coop Cooperativa de Consumo                                                                       |
| CoopCorreios                                                                                      |
| Cooperaliança                                                                                     |
| Cooperativa de Distribuição de Energia Fontoura Xavier - CERFOX                                   |
| Coopercocal                                                                                       |
| CooperJohnson                                                                                     |
| Coopernapi                                                                                        |
| Cooperzem Distruibuição                                                                           |
| Copa Airlines                                                                                     |
| Copanor                                                                                           |
| Copasa - Companhia de Saneamento de Minas Gerais                                                  |
| Copel - Companhia Paranaense de Energia                                                           |
| Cora                                                                                              |
| Correios                                                                                          |
| Corretora de Seguros Carrefour                                                                    |
| Cosesp                                                                                            |
| Cotação                                                                                           |
| CPFL Paulista                                                                                     |
| CPFL Piratininga                                                                                  |
| CPFL Santa Cruz                                                                                   |
| CredCrea                                                                                          |
| Credelesc                                                                                         |
| Crediare                                                                                          |
| Credicard                                                                                         |
| Credicenm                                                                                         |
| Credicitrus                                                                                       |
| Credicomin                                                                                        |
| Credifoz                                                                                          |
| Credipar                                                                                          |
| Creditaqui Financeira (Antiga Mercantil do Brasil Financeira)                                     |
| Creditas                                                                                          |
| Credsystem                                                                                        |
| Credz                                                                                             |
| Crefaz                                                                                            |
| Crefisa Seguros                                                                                   |
| Crefisa                                                                                           |
| Creluz-D                                                                                          |
| Cresol                                                                                            |
| Crevisc                                                                                           |
| Cruz Azul Saúde                                                                                   |
| CVC Viagens                                                                                       |
| Cyrela                                                                                            |
| D-Link                                                                                            |
| Dacasa Financeira                                                                                 |
| DAE Jundiaí                                                                                       |
| Dafiti                                                                                            |
| Dako                                                                                              |
| Danymotos                                                                                         |
| Darwin Seguros                                                                                    |
| Dasseg Seguros                                                                                    |
| Dayprev Vida e Previdência                                                                        |
| Decolar.com                                                                                       |
| Deezer                                                                                            |
| Dell                                                                                              |
| Delta Air Lines                                                                                   |
| Deltasul                                                                                          |
| Dental Uni                                                                                        |
| Departamento Municipal de Energia de Ijuí - DEMEI                                                 |
| Deville Hotéis                                                                                    |
| Dewalt                                                                                            |
| Dia Supermercados                                                                                 |
| Direcional Engenharia                                                                             |
| DIX Amico (DESATIVADA)                                                                            |
| DL Eletrônicos                                                                                    |
| DMCard                                                                                            |
| DME Distribuição                                                                                  |
| Docol Metais Sanitários                                                                           |
| Dog Hero                                                                                          |
| Dotz                                                                                              |
| Dpaschoal                                                                                         |
| Droga Raia                                                                                        |
| Drogal                                                                                            |
| Drogaria Nova Esperança                                                                           |
| Drogaria Onofre                                                                                   |
| Drogaria São Paulo                                                                                |
| Drogarias Pacheco                                                                                 |
| Drogasil                                                                                          |
| Dudalina (DESATIVADA)                                                                             |
| Dufrio                                                                                            |
| Duoflex Travesseiros                                                                              |
| Duracell                                                                                          |
| Eagle Sociedade de Crédito Direto                                                                 |
| Ebanx                                                                                             |
| EDE Editora e Distribuidora Educacional                                                           |
| Edelweiss Air                                                                                     |
| Editora Abril                                                                                     |
| Editora Escala                                                                                    |
| Editora Globo                                                                                     |
| Editora Moderna                                                                                   |
| EDP Espírito Santo                                                                                |
| EDP São Paulo                                                                                     |
| Educa Mais Brasil Programas Educacionais                                                          |
| Egali Intercâmbio                                                                                 |
| EL AL                                                                                             |
| Electrolux - Eletrodomésticos/Eletroportáteis                                                     |
| Electrolux - Eletroportáteis (DESATIVADA)                                                         |
| Eletro Aquila                                                                                     |
| Eletro Shopping (DESATIVADA)                                                                      |
| Eletrocar - Centrais Elétricas de Carazinho                                                       |
| Eletrosol Materiais Elétricos (DESATIVADA)                                                        |
| Eletrosom                                                                                         |
| Elevadores Atlas Schindler                                                                        |
| Ellus - Loja Online                                                                               |
| Elo7                                                                                              |
| Embasa                                                                                            |
| Emirates                                                                                          |
| Emotion Seguros                                                                                   |
| Empresa de Saneamento de Palestina - ESAP                                                         |
| Empresa Luz e Força Santa Maria - ELFSM                                                           |
| Enel Distribuição Ceará (Coelce)                                                                  |
| Enel Distribuição Goiás (CELG)                                                                    |
| Enel Distribuição Rio (Ampla)                                                                     |
| Enel Distribuição São Paulo (Eletropaulo)                                                         |
| Energisa Acre (Eletroacre)                                                                        |
| Energisa Borborema                                                                                |
| Energisa Mato Grosso do Sul                                                                       |
| Energisa Mato Grosso                                                                              |
| Energisa Minas Gerais                                                                             |
| Energisa Nova Friburgo                                                                            |
| Energisa Paraíba                                                                                  |
| Energisa Rondônia (Ceron)                                                                         |
| Energisa Sergipe                                                                                  |
| Energisa Sul-Sudeste                                                                              |
| Energisa Tocantins                                                                                |
| Enjoei                                                                                            |
| Época Cosméticos                                                                                  |
| Epson do Brasil                                                                                   |
| EQ Seguros (antiga Equatorial Seguradora)                                                         |
| Equatorial Alagoas                                                                                |
| Equatorial Amapá (CEA)                                                                            |
| Equatorial Maranhão (CEMAR)                                                                       |
| Equatorial Pará (CELPA)                                                                           |
| Equatorial Piauí                                                                                  |
| Equatorial Previdência                                                                            |
| Escola Metropolitana de Ribeirão Preto                                                            |
| Esmaltec                                                                                          |
| Essor Seguros                                                                                     |
| Estadão (Jornal O Estado de S. Paulo)                                                             |
| Ethiopian Airlines                                                                                |
| Etna                                                                                              |
| Eudora (Interbelle)                                                                               |
| Eurofarma                                                                                         |
| Eventim Brasil                                                                                    |
| Evidence Previdência                                                                              |
| Evino                                                                                             |
| Evolua                                                                                            |
| Excelsior Seguros                                                                                 |
| Expedia.com.br                                                                                    |
| Expresso Itamarati                                                                                |
| Extra.com                                                                                         |
| Ezze Seguros                                                                                      |
| Facebook / Instagram                                                                              |
| Facta Financeira                                                                                  |
| Facta Seguradora                                                                                  |
| Faculdade Anhanguera                                                                              |
| Faculdade FAMA                                                                                    |
| Faculdade Metropolitana de Ribeirão Preto                                                         |
| Faculdade Pitágoras                                                                               |
| Faculdade Unic                                                                                    |
| Faculdade UNIME                                                                                   |
| Faculdade Unopar                                                                                  |
| Fairfax Brasil                                                                                    |
| Fairway Seguros (antiga Travelers Seguros Brasil)                                                 |
| Farfetch                                                                                          |
| Farmamed Drogaria (DESATIVADA)                                                                    |
| Fast Shop                                                                                         |
| FATECE                                                                                            |
| Fator Seguradora                                                                                  |
| FEDAF-BR - Federação dos Agricultores na Agricultura Familiar do Brasil                           |
| Ferrero do Brasil                                                                                 |
| FFCred                                                                                            |
| Fiat Rivel - Blumenau, Brusque e Tijucas (DESATIVADA)                                             |
| Finamax                                                                                           |
| Financeira Alfa                                                                                   |
| Financeira BRB                                                                                    |
| Financeira Itaú Americanas (DESATIVADA - Atual Cartões Itaú)                                      |
| Fini - Loja Online                                                                                |
| Fini                                                                                              |
| Fischer                                                                                           |
| Flexform                                                                                          |
| Flybondi                                                                                          |
| Flytour Viagens                                                                                   |
| Fomento Paraná - Agência de Fomento do Paraná                                                     |
| Ford                                                                                              |
| Fortbrasil                                                                                        |
| Frigelar                                                                                          |
| Fundação Habitacional do Exército - FHE                                                           |
| Futon Company                                                                                     |
| Futuro Previdência Privada                                                                        |
| GA.MA Italy                                                                                       |
| Gama Saúde                                                                                        |
| Garantec (DESATIVADA - Atual Itaú Seguros)                                                        |
| Garoto                                                                                            |
| Gás Natural Serviços (Naturgy Soluções)                                                           |
| Gazin Seguros                                                                                     |
| Gazin.com.br                                                                                      |
| Gazincred                                                                                         |
| GBarbosa                                                                                          |
| GBOEX                                                                                             |
| GEAP (DESATIVADA)                                                                                 |
| General Mills (Yoki, Kitano, Mais Vita, Haagen Dazs, Betty Crocker)                               |
| Generali                                                                                          |
| Gente Seguradora                                                                                  |
| Gestora de Inteligência de Crédito                                                                |
| Getnet                                                                                            |
| Giga Gloob                                                                                        |
| Girafa.com.br                                                                                     |
| Globoplay                                                                                         |
| Gol Linhas Aéreas                                                                                 |
| Golden Cross                                                                                      |
| Gontijo                                                                                           |
| Google                                                                                            |
| Grand Cru                                                                                         |
| Grão de Gente                                                                                     |
| Gree                                                                                              |
| Grife Relógios                                                                                    |
| Grupo Editorial Beco dos Poetas &amp; Escritores                                                  |
| Guten Bier                                                                                        |
| GVT (DESATIVADA)                                                                                  |
| Harald                                                                                            |
| HDI Global Seguros                                                                                |
| HDI Seguros                                                                                       |
| Hello Study                                                                                       |
| Hering                                                                                            |
| Hershey's                                                                                         |
| Herval Móveis e Colchões                                                                          |
| Hidropan                                                                                          |
| Hipercard                                                                                         |
| Honda Automóveis                                                                                  |
| Hospital Águas Claras                                                                             |
| Hospital de Olhos Santa Luzia - Recife/PE                                                         |
| Hoteis.com                                                                                        |
| Hotmart                                                                                           |
| HP Brasil                                                                                         |
| HS Consórcios                                                                                     |
| HS Financeira                                                                                     |
| HS Seguros                                                                                        |
| HSBC (DESATIVADA)                                                                                 |
| Hub Pagamentos - Fintech Magalu                                                                   |
| Hughes                                                                                            |
| Hurb - Hotel Urbano                                                                               |
| Hyundai Caoa                                                                                      |
| Hyundai Motor Brasil                                                                              |
| Iberia Lineas Aereas                                                                              |
| Ibyte                                                                                             |
| Icatu Capitalização                                                                               |
| Icatu Seguros                                                                                     |
| IE Intercâmbio                                                                                    |
| iFood                                                                                             |
| Iguá Rio de Janeiro                                                                               |
| Ilunato                                                                                           |
| ImpressorAjato.com                                                                                |
| Incepa                                                                                            |
| Indiana Seguros                                                                                   |
| Infracommerce                                                                                     |
| Ingresso Rápido                                                                                   |
| Ingresso.com                                                                                      |
| Insinuante (DESATIVADA)                                                                           |
| Instituto Mix de Profissões                                                                       |
| Intelig                                                                                           |
| Interconnect Exchange Brasil                                                                      |
| Interep                                                                                           |
| Ipiranga                                                                                          |
| Iplace                                                                                            |
| ITA Bank (DESATIVADA)                                                                             |
| Italia Trasporto Aereo (ITA Airways)                                                              |
| Itambé                                                                                            |
| Itapemirim Transportes Aéreos (DESATIVADA)                                                        |
| Itapeva Recuperação de Créditos                                                                   |
| Itapoá Saneamento                                                                                 |
| Itaú Consignado                                                                                   |
| Itaú Consórcio                                                                                    |
| Itaú Seguros Auto e Residência                                                                    |
| Itaú Seguros                                                                                      |
| Itaú Unibanco Capitalização                                                                       |
| Itaú Unibanco Consignado                                                                          |
| Itaú Unibanco Crédito Imobiliário                                                                 |
| Itaú Vida e Previdência                                                                           |
| Itauleasing                                                                                       |
| Iu-á Hotel                                                                                        |
| Iugu                                                                                              |
| J17 Sociedade de Crédito Direto                                                                   |
| JBL Turismo                                                                                       |
| Jequiti Cosméticos                                                                                |
| JetSMART Airlines - Argentina                                                                     |
| JetSMART Airlines - Chile                                                                         |
| JNS Seguros                                                                                       |
| Jocar.com.br                                                                                      |
| Jocar                                                                                             |
| Jusbrasil                                                                                         |
| Just (DESATIVADA)                                                                                 |
| KaBuM!                                                                                            |
| Kalunga                                                                                           |
| Kanui                                                                                             |
| Kardbank                                                                                          |
| Karsten                                                                                           |
| Kellogg's / Parati                                                                                |
| Kian Iluminação                                                                                   |
| Kicaldo                                                                                           |
| KitchenAid                                                                                        |
| KLM                                                                                               |
| Koin                                                                                              |
| Komeco                                                                                            |
| Kovr Capitalização (antiga Invest Cap)                                                            |
| Kovr Previdência (antiga Investprev Seguros e Previdência)                                        |
| Kovr Seguradora (antiga Invest Seguradora)                                                        |
| L4B Logística (Loggi XD)                                                                          |
| Lactalis (Président Parmalat Elegê Batavo Cotochés Poços de Caldas Balkis Boa Nata Dobon Galbani) |
| Lalamove                                                                                          |
| Lancers Administradora                                                                            |
| Laser Eletro                                                                                      |
| Latam Airlines (Tam)                                                                              |
| Latam Cargo                                                                                       |
| Latam Travel                                                                                      |
| Latina Eletrodomésticos                                                                           |
| Lebes.com.br                                                                                      |
| Lecca CFI                                                                                         |
| Lenovo Celular (DESATIVADA)                                                                       |
| Lenovo Informática                                                                                |
| Leroy Merlin                                                                                      |
| Leve Saúde                                                                                        |
| Lexmark                                                                                           |
| LG Electronics                                                                                    |
| Liberty Seguros                                                                                   |
| Liderança Capitalização (Tele Sena)                                                               |
| Liga Invest (Antiga Lecca DTVM)                                                                   |
| Ligga Telecom (antiga Copel Telecom)                                                              |
| Light                                                                                             |
| Linca - Net Foz                                                                                   |
| Linea                                                                                             |
| Livelo                                                                                            |
| Livepass                                                                                          |
| Living                                                                                            |
| Livraria Cultura                                                                                  |
| Localiza Meoo                                                                                     |
| Localiza Rent a Car                                                                               |
| Localiza Seminovos                                                                                |
| Localiza Zarp                                                                                     |
| Loft                                                                                              |
| Loggi                                                                                             |
| Loja Brastemp                                                                                     |
| Loja Brazil                                                                                       |
| Loja Consul                                                                                       |
| Loja Electrolux                                                                                   |
| Lojas Avenida                                                                                     |
| Lojas Becker                                                                                      |
| Lojas Benoit                                                                                      |
| Lojas Cem                                                                                         |
| Lojas Colombo.com                                                                                 |
| Lojas Colombo                                                                                     |
| Lojas Dona do Lar                                                                                 |
| Lojas Koerich                                                                                     |
| Lojas Lebes                                                                                       |
| Lojas Marisa                                                                                      |
| Lojas MM - Mercadomóveis                                                                          |
| Lojas Quero-Quero                                                                                 |
| Lojas Renner                                                                                      |
| Lojas Riachuelo                                                                                   |
| Lojas Salfer (DESATIVADA)                                                                         |
| Lojas Sipolatti                                                                                   |
| Lojas TaQi                                                                                        |
| Loréal Brasil                                                                                     |
| Losango                                                                                           |
| Lufthansa                                                                                         |
| Luizacred                                                                                         |
| Luizaseg                                                                                          |
| Luxottica                                                                                         |
| MadeiraMadeira                                                                                    |
| Maga +                                                                                            |
| Magazine Luiza - Loja Física                                                                      |
| Magazine Luiza - Loja Online                                                                      |
| Magic City                                                                                        |
| Malwee                                                                                            |
| MAP Linhas Aéreas                                                                                 |
| Mapfre Capitalização                                                                              |
| Mapfre Consórcios                                                                                 |
| Mapfre Investimentos                                                                              |
| Mapfre Previdência                                                                                |
| Mapfre Saúde                                                                                      |
| Mapfre Seguros                                                                                    |
| Mapfre Vida                                                                                       |
| Marabraz - Lojas Físicas                                                                          |
| Marabraz.com                                                                                      |
| Marelli - Móveis para Escritório                                                                  |
| Marfrig                                                                                           |
| Marisa - Loja Online                                                                              |
| Mars Brasil (Masterfoods)                                                                         |
| Mastercard Brasil                                                                                 |
| Maternidade Brasília                                                                              |
| MaxMilhas                                                                                         |
| Maxxi Atacado                                                                                     |
| MBM Previdência Complementar                                                                      |
| MBM Seguros                                                                                       |
| MegaMamute                                                                                        |
| Menu Móveis                                                                                       |
| Mercadinhos São Luiz - Ceará                                                                      |
| Mercado Extra                                                                                     |
| Mercado Livre                                                                                     |
| Mercado Pago                                                                                      |
| Mercadorama                                                                                       |
| Mercantil Rodrigues                                                                               |
| Metlife Administradora de Fundos Multipatrocinados (MultiPrev)                                    |
| Metlife Planos Odontológicos                                                                      |
| Metlife Seguros e Previdência Privada                                                             |
| MeuFluxo                                                                                          |
| MG Seguros                                                                                        |
| MGW Ativos                                                                                        |
| Midway (Riachuelo)                                                                                |
| Mistertech                                                                                        |
| Mitsui Sumitomo Seguros                                                                           |
| Mmartan                                                                                           |
| Mobilize Financial Services/Credi Nissan (Antigo Banco Renault)                                   |
| Mobly                                                                                             |
| modalmais                                                                                         |
| Moip / Wirecard                                                                                   |
| Momenta Farmacêutica                                                                              |
| Mondial                                                                                           |
| Monetizze                                                                                         |
| Mongeral Aegon Seguros e Previdência - MAG                                                        |
| mony mony                                                                                         |
| Moto Honda                                                                                        |
| Motorola                                                                                          |
| Move Mais                                                                                         |
| Móveis Gazin                                                                                      |
| Móveis Romera                                                                                     |
| MRV Engenharia                                                                                    |
| Mueller Eletrodomésticos                                                                          |
| Mueller Fogões                                                                                    |
| Multi B                                                                                           |
| Multilaser                                                                                        |
| Multimarcas Consórcios                                                                            |
| Multiplus (DESATIVADA)                                                                            |
| Mundial Editora                                                                                   |
| Mux Energia                                                                                       |
| Nacional                                                                                          |
| Nadir Figueiredo - Marinex - Copo Americano - SM - Duralex - Colorex - Sempre                     |
| Natália Jóias                                                                                     |
| Natura                                                                                            |
| Naturgy (antiga Gás Natural Fenosa)                                                               |
| NBC Bank                                                                                          |
| Neoenergia Brasília (CEB)                                                                         |
| Neoenergia Coelba                                                                                 |
| Neoenergia Cosern                                                                                 |
| Neoenergia Elektro                                                                                |
| Neoenergia Pernambuco (Celpe)                                                                     |
| Nescafé Dolce Gusto                                                                               |
| Nespresso                                                                                         |
| Nestlé                                                                                            |
| Net Angra                                                                                         |
| Net Catanduva (DESATIVADA)                                                                        |
| NET                                                                                               |
| Netbabys                                                                                          |
| Netfarma (DESATIVADA)                                                                             |
| Netflix                                                                                           |
| Netshoes                                                                                          |
| Newe Seguros                                                                                      |
| Nextel (DESATIVADA - atual Claro)                                                                 |
| Nissan                                                                                            |
| Norwegian (DESATIVADA)                                                                            |
| NotreDame Intermédica                                                                             |
| Nova Fibra Telecom                                                                                |
| Novo Mundo                                                                                        |
| Nubank                                                                                            |
| Numbrs                                                                                            |
| O Boticário (Interbelle)                                                                          |
| Odonto Empresas                                                                                   |
| Odontoprev                                                                                        |
| OfficeTotalShop                                                                                   |
| Oi Celular                                                                                        |
| Oi Fixo                                                                                           |
| Oi Paggo Administradora de Crédito                                                                |
| OLX Pay                                                                                           |
| OLX                                                                                               |
| Omint Seguros                                                                                     |
| Omni Financeira                                                                                   |
| OMNI Informática                                                                                  |
| Oster do Brasil                                                                                   |
| Óticas Carol                                                                                      |
| OUi Paris                                                                                         |
| Ourocard                                                                                          |
| OvelhaNegraMusical.com.br                                                                         |
| Oxxy Seguradora                                                                                   |
| Pagseguro                                                                                         |
| Palácio das Ferramentas                                                                           |
| Panasonic                                                                                         |
| Panini                                                                                            |
| Panvel Farmácias                                                                                  |
| Pão de Açúcar                                                                                     |
| Paraná Banco                                                                                      |
| Paranaguá Saneamento                                                                              |
| Paranair                                                                                          |
| Parati Financeira                                                                                 |
| ParPerfeito                                                                                       |
| Passaredo Linhas Aéreas                                                                           |
| Pater Seguros                                                                                     |
| Paulista Serviços                                                                                 |
| PayPal do Brasil                                                                                  |
| Pecúlio União Previdência Privada                                                                 |
| Pefisa                                                                                            |
| Peixe Urbano                                                                                      |
| Perfu.me (DESATIVADA)                                                                             |
| Perini                                                                                            |
| Perkus                                                                                            |
| Pernambucanas Cartões                                                                             |
| Pernambucanas                                                                                     |
| Petlove                                                                                           |
| Philco                                                                                            |
| Philips Áudio e Vídeo                                                                             |
| Philips Avent e Cuidados Pessoais                                                                 |
| Philips TV e Monitores                                                                            |
| Philips Walita                                                                                    |
| PicPay                                                                                            |
| Pier Seguradora                                                                                   |
| Pincred                                                                                           |
| Piracanjuba                                                                                       |
| Pitzi                                                                                             |
| Plena Saúde                                                                                       |
| PointGres (DESATIVADA – atual Perkus)                                                             |
| Polishop.com.br                                                                                   |
| Polishop                                                                                          |
| Politorno (DESATIVADA)                                                                            |
| Ponto Frio                                                                                        |
| Pontofrio.com                                                                                     |
| Portal Terra                                                                                      |
| Porto Seguro Capitalização                                                                        |
| Porto Seguro Cartões e Financiamentos                                                             |
| Porto Seguro Cia de Seguros Gerais                                                                |
| Porto Seguro Consórcios                                                                           |
| Porto Seguro Previdência                                                                          |
| Porto Seguro Saúde e Odonto                                                                       |
| Porto.Pet                                                                                         |
| Portocred Financeira                                                                              |
| Positivo Tecnologia                                                                               |
| Postos Petrobras (Vibra Energia)                                                                  |
| Postos Shell                                                                                      |
| Pottencial Seguradora                                                                             |
| Poupecredi                                                                                        |
| Poupex                                                                                            |
| Premiere                                                                                          |
| Previmax Previdência Privada e Seguradora                                                         |
| Previmil Vida e Previdência                                                                       |
| Previsul                                                                                          |
| Prezunic                                                                                          |
| Privalia                                                                                          |
| Procorrer                                                                                         |
| Procter &amp; Gamble (P&amp;G)                                                                    |
| Prudential do Brasil Vida em Grupo                                                                |
| Prudential do Brasil                                                                              |
| Puma                                                                                              |
| Purificador de Água Brastemp                                                                      |
| Purina                                                                                            |
| Qatar Airways                                                                                     |
| QBE Brasil Seguros (DESATIVADA – atual Zurich Seguros)                                            |
| QI Escolas                                                                                        |
| QI Tech                                                                                           |
| Qsaúde                                                                                            |
| Qualicorp                                                                                         |
| quem disse, berenice?                                                                             |
| QuintoAndar                                                                                       |
| Randon Administradora de Consórcios                                                               |
| Rappi                                                                                             |
| Rappipay                                                                                          |
| RecargaPay                                                                                        |
| Recíproca Assistência                                                                             |
| Recovery do Brasil Consultoria                                                                    |
| Rede                                                                                              |
| Renault On Demand                                                                                 |
| Renault                                                                                           |
| RGE - Rio Grande Energia                                                                          |
| Riachuelo - Loja Online                                                                           |
| Ricardo Eletro.com (DESATIVADA)                                                                   |
| Ricardo Eletro                                                                                    |
| Richards - Loja Online                                                                            |
| Rico Investimentos                                                                                |
| Rinnai                                                                                            |
| Rio Grande Capitalização                                                                          |
| Rio Grande Seguros e Previdência                                                                  |
| Rio Mais Saneamento                                                                               |
| Rio Verde Engenharia                                                                              |
| Rio's Capitalização (Antiga SulAmérica Capitalização)                                             |
| Roca                                                                                              |
| Rodobens Consórcio                                                                                |
| Roraima Energia                                                                                   |
| Rota Perdida                                                                                      |
| Royal Air Maroc                                                                                   |
| Royal Canin do Brasil                                                                             |
| SAAE Sorocaba                                                                                     |
| Saapi                                                                                             |
| Sabemi Previdência Privada                                                                        |
| Sabemi Seguradora                                                                                 |
| Sabesp                                                                                            |
| Safra Seguros Gerais                                                                              |
| Safra Vida e Previdência                                                                          |
| Salinas - Loja Online                                                                             |
| Salvador Norte Shopping                                                                           |
| Salvador Shopping                                                                                 |
| Sam´s Club                                                                                        |
| Sami Saúde                                                                                        |
| Samsung - Loja Online                                                                             |
| Samsung                                                                                           |
| Sancor Seguros                                                                                    |
| Saneago                                                                                           |
| Saneamento de Mirassol - Sanessol                                                                 |
| Sanepar                                                                                           |
| Santa Helena Saúde                                                                                |
| Santander Auto                                                                                    |
| Santander Capitalização                                                                           |
| Santander Financiamentos (Aymoré)                                                                 |
| Santher                                                                                           |
| Santillana Educação                                                                               |
| Santinvest                                                                                        |
| Santista                                                                                          |
| Score Group                                                                                       |
| Seguradora Líder dos Consórcios do Seguro DPVAT                                                   |
| Seguros Atual                                                                                     |
| Seguros Honda                                                                                     |
| Seguros Sura                                                                                      |
| Sem Parar Empresas (VB Serviços)                                                                  |
| Sem Parar                                                                                         |
| SEMP TCL                                                                                          |
| Sempre Odonto                                                                                     |
| Senac - Santa Catarina                                                                            |
| Senff                                                                                             |
| Serasa Experian                                                                                   |
| Seven Boys                                                                                        |
| SF3 (Santana Financeira)                                                                          |
| Shipp - Americanas Delivery                                                                       |
| Shopee Brasil                                                                                     |
| Shopfato.com                                                                                      |
| Shopping Jardins Aracaju                                                                          |
| Shopping Riomar Aracaju                                                                           |
| Shopping Riomar Fortaleza                                                                         |
| Shopping Riomar Kennedy                                                                           |
| Shopping Riomar Recife                                                                            |
| Shoptime Viagens                                                                                  |
| Shoptime                                                                                          |
| Sicoob Seguradora                                                                                 |
| Sicredi                                                                                           |
| Sinaf Seguros                                                                                     |
| Sindicato Nacional dos Condutores da Marinha Mercante e Afins - SINCOMAM                          |
| Single Care                                                                                       |
| Sinosserra Financeira                                                                             |
| Sinosserra Seguros                                                                                |
| SJ Administração de Imóveis - Ceará                                                               |
| Sky Airline                                                                                       |
| SKY                                                                                               |
| Smartfit                                                                                          |
| Smiles                                                                                            |
| SOBAM Centro Médico                                                                               |
| Social Bank                                                                                       |
| Sociedade Caxiense de Mútuo Socorro                                                               |
| Soluções Moderna                                                                                  |
| Sombrero Seguros                                                                                  |
| Sompo Seguros                                                                                     |
| Sonda Supermercados                                                                               |
| Sorriso Operadora Odontológica                                                                    |
| Sou barato                                                                                        |
| South African Airways                                                                             |
| SPC Brasil                                                                                        |
| Spirit Ventiladores                                                                               |
| Stanley                                                                                           |
| Starbucks @ home                                                                                  |
| Starr Brasil Seguradora                                                                           |
| STB - Student Travel Bureau                                                                       |
| STB Travel Shop                                                                                   |
| Stix                                                                                              |
| Studio Z                                                                                          |
| Submarino Viagens                                                                                 |
| Submarino                                                                                         |
| Sudacred                                                                                          |
| Sudaseg Seguradora                                                                                |
| SudaVida (antiga Sudamerica Vida)                                                                 |
| Suhai Seguradora                                                                                  |
| Suíça Seguradora                                                                                  |
| SulAmérica Auto, Residencial, Empresarial e Condomínio (DESATIVADA - Atual Allianz Seguros)       |
| SulAmérica Odontológico                                                                           |
| SulAmérica Saúde                                                                                  |
| SulAmérica Seguradora de Saúde (Antiga Sompo Saúde)                                               |
| SulAmérica Seguros de Pessoas e Previdência                                                       |
| SULGIPE – Companhia Sul Sergipana de Eletricidade                                                 |
| SumUp                                                                                             |
| Sunglass Hut                                                                                      |
| Super do Povo                                                                                     |
| Supera Farma                                                                                      |
| Superbompreço                                                                                     |
| Supermed                                                                                          |
| Supermercadinhos Santa Rita - Ceará                                                               |
| Supermercado Nidobox                                                                              |
| Supermercado Now                                                                                  |
| Supermercados Mundial                                                                             |
| Supermercados Super Lagoa                                                                         |
| SuperMuffato.com                                                                                  |
| Surinam Airways                                                                                   |
| Swiss Re Corporate Solutions Brasil Seguros                                                       |
| Swiss                                                                                             |
| Sympla                                                                                            |
| Systemcred                                                                                        |
| TAAG - Linhas Aéreas de Angola                                                                    |
| TACA Airlines                                                                                     |
| TAP Air Portugal                                                                                  |
| Telecine                                                                                          |
| Telhanorte                                                                                        |
| Termolar                                                                                          |
| The Body Shop                                                                                     |
| Ticket360                                                                                         |
| TicketMaster Brasil                                                                               |
| Tickets For Fun                                                                                   |
| TIGRE                                                                                             |
| Tim                                                                                               |
| Titânia Telecom                                                                                   |
| Todo Dia Supermercados                                                                            |
| Tok&amp;Stok - Loja Online                                                                        |
| Tokbelas                                                                                          |
| Tokio Marine                                                                                      |
| Too Seguros (antiga PAN Seguros)                                                                  |
| Toyota                                                                                            |
| Toyoville                                                                                         |
| Traditio Companhia de Seguros                                                                     |
| Transpocred                                                                                       |
| Travelmate Intercâmbio &amp; Turismo                                                              |
| Tribanco Seguros                                                                                  |
| Tribanco                                                                                          |
| Tricae                                                                                            |
| Tricard                                                                                           |
| Trigg                                                                                             |
| TTL Transportes                                                                                   |
| Tudo Azul                                                                                         |
| Tudus                                                                                             |
| Turbo Fitness                                                                                     |
| Turkish Airlines                                                                                  |
| Two Flex Aviação Inteligente                                                                      |
| Uber                                                                                              |
| UCB Biopharma                                                                                     |
| Ultrafarma                                                                                        |
| Ultragaz                                                                                          |
| Unesul                                                                                            |
| União Seguradora                                                                                  |
| Uniconsult Administradora de Benefícios                                                           |
| Unicred                                                                                           |
| Unidas Rent a Car                                                                                 |
| Unifique                                                                                          |
| Unilos                                                                                            |
| Unimed Apucarana                                                                                  |
| Unimed Assis                                                                                      |
| Unimed Belém                                                                                      |
| Unimed Blumenau                                                                                   |
| Unimed Brusque                                                                                    |
| Unimed Cariri                                                                                     |
| Unimed Cascavel                                                                                   |
| Unimed Centro Oeste Paulista                                                                      |
| Unimed Costa Oeste                                                                                |
| Unimed Curitiba                                                                                   |
| Unimed de Pindamonhangaba                                                                         |
| Unimed de Sobral                                                                                  |
| Unimed de Votuporanga                                                                             |
| Unimed Divinópolis                                                                                |
| Unimed do Ceará                                                                                   |
| Unimed do Oeste do Paraná                                                                         |
| Unimed Fesp                                                                                       |
| Unimed Fortaleza                                                                                  |
| Unimed Grande Florianópolis                                                                       |
| Unimed Guarapuava                                                                                 |
| Unimed Litoral                                                                                    |
| Unimed Londrina                                                                                   |
| Unimed Maceió                                                                                     |
| Unimed Maranhão do Sul                                                                            |
| Unimed Marquês de Valença                                                                         |
| Unimed Nacional (Central Nacional Unimed)                                                         |
| Unimed Noroeste do Paraná                                                                         |
| Unimed Noroeste/RS                                                                                |
| Unimed Norte Nordeste                                                                             |
| Unimed Norte Pioneiro - PR                                                                        |
| Unimed Nova Iguaçu                                                                                |
| Unimed Odonto                                                                                     |
| Unimed Paraná                                                                                     |
| Unimed Pato Branco (DESATIVADA)                                                                   |
| Unimed Patos de Minas                                                                             |
| Unimed Paulistana (DESATIVADA)                                                                    |
| Unimed Piracicaba                                                                                 |
| Unimed Planalto Norte de Santa Catarina                                                           |
| Unimed Porto Alegre                                                                               |
| Unimed Presidente Prudente                                                                        |
| Unimed Regional Campo Mourão                                                                      |
| Unimed Ribeirão Preto                                                                             |
| Unimed Santa Catarina                                                                             |
| Unimed São José do Rio Preto                                                                      |
| Unimed São José dos Campos                                                                        |
| Unimed Seguradora                                                                                 |
| Unimed Seguros Patrimoniais                                                                       |
| Unimed Seguros Saúde                                                                              |
| Unimed Sorocaba                                                                                   |
| Unimed Sudoeste Paulista                                                                          |
| Unimed Três Rios                                                                                  |
| Unimed Uberlândia                                                                                 |
| Unimed Vale do Sepotuba                                                                           |
| Unimed/RS Federação                                                                               |
| United Airlines                                                                                   |
| Universidade Anhembi Morumbi                                                                      |
| Universidade Estácio de Sá                                                                        |
| Universo Online - UOL                                                                             |
| UP Promotora                                                                                      |
| UP.P                                                                                              |
| UPL do Brasil                                                                                     |
| UPOFA - União Previdencial                                                                        |
| Usebens Seguradora                                                                                |
| UY3 Sociedade de Crédito Direto                                                                   |
| Uze                                                                                               |
| Valejet.com                                                                                       |
| Valor Financiamentos                                                                              |
| Vanguardacap Capitalização                                                                        |
| Venturi, Falmec, Smeg e Arix                                                                      |
| Via Capitalização (antiga APLUB Capitalização)                                                    |
| Via Certa Financiadora                                                                            |
| Viação Continental                                                                                |
| Viação Santa Cruz                                                                                 |
| Viacredi Alto Vale                                                                                |
| Viacredi                                                                                          |
| ViajaNet                                                                                          |
| Viasat                                                                                            |
| Vip Telecom                                                                                       |
| Virgin Atlantic Airways (DESATIVADA)                                                              |
| Virginia Surety Seguros                                                                           |
| Vistual (DESATIVADA)                                                                              |
| Vitreo DTVM                                                                                       |
| Viva Air                                                                                          |
| Vivara                                                                                            |
| Vivaz                                                                                             |
| Viver Previdência                                                                                 |
| Vivo - Telefônica                                                                                 |
| Volkswagen                                                                                        |
| voulevar                                                                                          |
| VR Collezioni - Loja Online                                                                       |
| Vult Cosmética                                                                                    |
| Walmart (DESATIVADA)                                                                              |
| Walmart.com (DESATIVADA)                                                                          |
| WAP - Fresnomaq                                                                                   |
| WebContinental                                                                                    |
| Wickbold                                                                                          |
| Will Bank (antigo Meu Pag!)                                                                       |
| Wine                                                                                              |
| Wiser Educação (Wise Up, Number One, MeuSucesso.com e Buzz Editora)                               |
| World Study                                                                                       |
| XP Investimentos                                                                                  |
| XP Vida e Previdência                                                                             |
| XS2 Vida e Previdência                                                                            |
| XS4 Capitalização                                                                                 |
| Youcom                                                                                            |
| Youse Seguradora                                                                                  |
| Zap Imóveis                                                                                       |
| Zattini                                                                                           |
| Zema Financeira                                                                                   |
| Zema Seguros                                                                                      |
| Zoom                                                                                              |
| Zoop                                                                                              |
| Zurich Capitalização                                                                              |
| Zurich Santander Seguros                                                                          |
| Zurich Santander Vida e Previdência                                                               |
| Zurich Seguros                                                                                    |
| Zurich Vida e Previdência                                                                         |

#### regiao (int or str)
ID da região de origem da reclamação.

#### uf (int or str)
ID do estado de origem da reclamação.

#### cidade (int or str)
ID da cidade de origem da reclamação.

#### area (str)
Área de atuação da empresa.  
==NOT WORKING==

#### assunto (str)
Assunto, ou seja, produto ou serviço origem do problema.  
==NOT WORKING==

#### problema (str)
Categorização do problema.  
==NOT WORKING==

#### dataInicio (str)
Data inicial do período de busca. A data refere-se à resposta da empresa.  
Formato dd/MM/yyyy.

#### dataTermino (str)
Data final do período de busca. A data refere-se à resposta da empresa.  
Formato dd/MM/yyyy.

#### avaliacao (int or str)
ID da avaliação do cliente, quanto à resolução dada pela empresa para o problema relatado.

| ID  | Avaliação do cliente |
| --- | -------------------- |
| 1   | Resolvida            |
| 2   | Não Resolvida        |
| 3   | Não Avaliada         |

#### nota (int or str)
Nota de 1 a 5 dada pelo cliente à resposta da empresa.

## Performance
O custo computacional de aquisição dos dados medido nos testes foi de **1 segundo a cada 10 linhas buscadas**, quando o único parâmetro de filtro passado é a palavra chave. **Para mais filtros, o tempo de busca pode ser maior**.

Se desejar limitar o tempo de busca, basta passar o parâmetro `numero_maximo_resultados`.