# AI Dokumentu Apstrādes Sistēma - Iestatīšanas Instrukcija

## Prasības pirms darbības uzsākšanas

### 1. **OpenAI API atslēgas**
- Nepieciešamas OpenAI API atslēgas visiem darba plūsmām
- Jākonfigurē OpenAI piekļuves dati N8N sistēmā
- Izmantotais modelis: `gpt-4.1-mini` vai jaunāks

### 2. **Function Server (OBLIGĀTI AKTĪVS)**
- `FunctionServer.json` darba plūsma **VIENMĒR** jābūt aktīvai
- Šī darba plūsma nodrošina MCP servera funkcionalitāti
- Bez aktīvas Function Server galvenais aģents nestrādās

### 3. **MCP Servera Galapunkti**
- `MainAgent.json` darba plūsmā jāatjaunina MCP servera galapunkti
- Jāpārbauda un jāmaina galapunktu ID atbilstoši jūsu Function Server webhook ID

## Darba plūsmu apraksts

### 1. **Metadata.json** - Dokumentu Apstrādes Darba plūsma
- **Mērķis**: Apstrādā un saglabā dokumentus vektoru datubāzē
- **Atbalstītie formāti**: PDF, DOCX, XLSX
- **Funkcijas**:
  - Lasa dokumentus no `/data/downloads/invoice_data_2024/` (vai var pārmainīt uz citu lokāla diska direktoriju)
  - Ģenerē failu hash duplikātu pārbaudei
  - Ekstraktē tekstu no dažādiem formātiem
  - Izmanto OpenAI kopsavilkumu izveidei dokumentu analīzei
  - Saglabā Qdrant vektoru datubāzē ar iegulšanu vektoriem

### 2. **Vectordatabase.json** - Meklēšanas Darba plūsma
- **Mērķis**: Veic dokumentu meklēšanu vektoru datubāzē
- **Funkcijas**:
  - Ģenerē vaicājumu iegulšanas vektorus ar OpenAI
  - Meklē līdzīgus dokumentus Qdrant datubāzē
  - Atgriež rezultātus ar/bez pilna satura (PageContent)
  - Optimizē veiktspēju atkarībā no meklēšanas tipa

### 3. **FunctionServer.json** - MCP Serveris
- **Mērķis**: Nodrošina AI aģenta funkcionalitāti
- **KRITISKĀ PRASĪBA**: Vienmēr jābūt aktīvam
- **Funkcijas**:
  - `FastDocumentSearch` - ātrai dokumentu atrašanai
  - `DetailedContentAnalysis` - detalizētai satura analīzei

### 4. **MainAgent.json** - Galvenais AI Aģents
- **Mērķis**: Lietotāja saskarsmes punkts
- **Atkarības**: Function Server jābūt aktīvam
- **Funkcijas**:
  - Dokumentu meklēšana un analīze
  - Aprēķini ar Calculator rīku
  - Atmiņas saglabāšana sarunu starpā

## Darbības secība

1. **Sākumā palaidiet**: `FunctionServer.json` (aktīvs statuss)
2. **Apstrādājiet dokumentus**: `Metadata.json` 
3. **Atjauniniet galapunktus**: `MainAgent.json`
4. **Sāciet darbu**: `MainAgent.json` tērzēšanas saskarsme

## Svarīgas piezīmes

- Qdrant datubāze jābūt pieejama portā 6333
- OpenAI iegulšanas izmanto `text-embedding-3-small` modeli
- Visas darba plūsmas izmanto tos pašus Qdrant piekļuves datus
- Function Server webhook ID jāatbilst MCP galapunktu konfigurācijai

## Problēmu novēršana

- Ja aģents neatbild: pārbaudiet Function Server statusu
- Ja nav meklēšanas rezultātu: pārbaudiet, vai Metadata darba plūsma ir apstrādājusi dokumentus
- Ja MCP kļūdas: pārbaudiet galapunktu URL un webhook ID atbilstību
