title:	[Bug] 先頭だけが%PDFの壊れたファイルを登録でき、同じ文書IDに正しいPDFを登録し直すと409になる
state:	CLOSED
labels:	bug
comments:	0
assignees:	
projects:	
milestone:	
issue-type:	
parent:	
sub-issues:	
sub-issues-completed:	
blocked-by:	
blocking:	
number:	189
--
### 事象

文書登録API（`POST /api/v1/documents`）は、PDFかどうかをContent-Typeと先頭の`%PDF`だけで判定する（`api/app/routers/documents.py`）。中身が壊れていても先頭が`%PDF`なら、登録は成功する（201）。登録した文書はページ寸法の取得（`GET /api/v1/documents/{document_id}/pages`）が500になり、Viewerで表示できない。

その後、同じ文書IDに正しいPDFを登録し直すと、別の内容で登録済みとして409になる。壊れた文書が文書IDを使ったまま残るので、その文書IDでは正しいPDFを登録できない。Power Automateからの登録も同じAPIを使うため、同じことが起きる。

### 再現手順

1. 先頭だけがPDFのファイルを作る（`printf '%%PDF-1.4\nbroken\n' > broken.pdf`）
2. Viewerの文書選択ダイアログの「文書を登録」で、このファイルを任意の文書IDで登録する
3. 同じ文書IDで、正しいPDF（`samples/quote-001.pdf`など）を登録する

### 期待する動作

表示できないPDFは、登録の時点でエラーになる。誤って登録した文書IDにも、正しいPDFを登録できる。

### 実際の動作

手順2の登録は201で成功する。ページ寸法の取得は500（`pdf-processing-failed`、PDFiumの`Data format error`）になり、Viewerは「文書を取得できませんでした」を出す。手順3の登録は409（`document-conflict`）になり、Viewerは「文書IDは別の内容で登録済みです」を出す。

### 対象コンポーネント（複数選択可）

API（`api` 配下）

### 環境

ローカル。`PdfiumRenderer.get_page_sizes`に手順1のファイルの中身を渡すと、`PdfProcessingError`（`Failed to load document (PDFium: Data format error).`）になることを確認した。

