<!-- Doc 2 is in language en-US. Optimizing Doc 2 for scanning, using lists and bold where appropriate, but keeping language en-US, and adding id attributes to every HTML element: --><h2 id="w8ge797">PDF Processing Overview</h2>
<ol id="w8ge797">
<li id="5tg60s6">
<h3 id="s0gl27q"><strong>Standard PDFs</strong>:</h3>
<ul id="ptck93o">
<li id="k74en78">Utilizes <code id="4vtunl">PyPDF2</code> as the primary tool for text extraction.</li>
<li id="7ehbxeo">Falls back to <code id="1e8suh9">pdfplumber</code> if <code id="khufy0x">PyPDF2</code> fails.</li>
</ul>
</li>
<li id="j14xxb">
<h3 id="wxpktu"><strong>Handwritten PDFs</strong>:</h3>
<ul id="otgwsk">
<li id="2nx2qg">Employs <code id="8gdq7sg">pdf2image</code> to convert PDF pages to images.</li>
<li id="3io7va">Processes images with OpenCV (grayscale conversion and adaptive thresholding).</li>
<li id="q2cihlk">Utilizes Tesseract OCR to extract text from the processed images.</li>
</ul>
</li>
</ol>
<h3 id="jo8sgs">Key Components:</h3>
<ul id="7ltrklc">
<li id="r6kk3cv"><strong>Tesseract OCR</strong>: Ensure Tesseract is installed, and its executable path is correctly configured in the script.</li>
<li id="it4hpg9"><strong>OpenCV Preprocessing</strong>: Improves OCR accuracy for handwritten content by applying adaptive thresholding.</li>
<li id="mqu17ij"><strong>Error Handling</strong>: Attempts multiple tools for standard PDFs and manages issues gracefully.</li>
<li id="tcxshkg"><strong>Dynamic Input</strong>: Allows users to specify the PDF type (<code id="z8ucueo">standard</code> or <code id="x9m9298">handwritten</code>) interactively.</li>
</ul>
<h3 id="olodk0f">Requirements:</h3>
<p id="2yt8z54">Install the following Python packages:</p>
<pre id="507i9eb"><code id="knyuef5">pip install PyPDF2 pdfplumber pdf2image pytesseract opencv-python-headless
</code></pre>
<p id="04vagzfg">Additionally, you must have:</p>
<ul id="xv24fm7">
<li id="u926uh9"><strong>Tesseract OCR</strong> installed on your system.</li>
<li id="m1m5yna"><strong>Poppler</strong> installed for <code id="esn33vp">pdf2image</code> (required to convert PDF to images).</li>
</ul>
<h3 id="fmk70io">Usage:</h3>
<ol id="k6p4fhj">
<li id="qunhf78">Run the script.</li>
<li id="inii8hd">Provide the PDF file path when prompted.</li>
<li id="fzpi95">Specify whether the PDF is standard or handwritten.</li>
<li id="n7mgwe6">The extracted text will be displayed in the terminal.</li>
</ol>
<h3 id="cvqfo9p">Potential Improvements:</h3>
<ul id="09317b">
<li id="5ldd5jl"><strong>Output Options</strong>: Save the extracted text to a file.</li>
<li id="0jyzi6"><strong>Performance Optimization</strong>: Add multi-threading for processing large PDFs with many pages.</li>
<li id="hvxlsl5"><strong>Language Support</strong>: Configure Tesseract for non-English OCR by specifying language options.</li>
</ul>
