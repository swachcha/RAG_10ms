1.What method or library did you use to extract the text, and why? Did you face any formatting challenges with the PDF content?
PyPDF2 for normal PDFs and Tesseract OCR for scanned PDFs.Tesseract needed special training for Bengali letters. 
Problems faced:
Sometimes Bengali letters stuck together.
Scanned pages had text in wrong places.

2.What chunking strategy did you choose (e.g. paragraph-based, sentence-based, character limit)? Why do you think it works well for semantic retrieval?
I used paragraph first chunking with fallbacks to Bengali sentence breaks (using stopword like ।) and 400-character chunks with overlap. This works because paragraphs 
keep complete ideas together, while the । punctuation handles Bengali's unique sentence structure better than periods. The 400 character size fits most 
answers while the overlap prevents cut off responses. For example, questions like "অনুপমের ভাষায় সুপুরুষ কাকে বলা হয়েছে?" reliably find answers because 
chunking preserves full thoughts. This approach balances precision with context better than fixed size chunks alone.

3. What embedding model did you use? Why did you choose it? How does it capture the meaning of the text?
I used the bge-small-bn embedding model because it's specifically designed to work with both Bengali and English text. The reason I picked this one is
simple it just understands Bengali better than the other options I tried. It's really good at figuring out when different words mean similar things, like 
knowing that "বিদ্যালয়" and "স্কুল" are basically the same, or that "বৃষ্টি" in Bengali matches with "rain" in English. What I like about it is that it's not too 
big or slow, but still gets the job done accurately.

4. How are you comparing the query with your stored chunks? Why did you choose this similarity method and storage setup?
When comparing questions to the stored text chunks, I use a system called FAISS that's really fast at finding matches. I chose cosine similarity because it's great at
measuring how closely the meaning of the question matches the meaning of the text chunks, rather than just looking for exact word matches. The storage is set up to keep 
Bengali and English content organized separately but still lets them work together when needed. This setup makes the whole system quick to respond while still understanding 
what you're really asking, even if you phrase your question differently than how the answer appears in the text.

5. How do you ensure that the question and the document chunks are compared meaningfully? What would happen if the query is vague or missing context?
To make sure questions match properly with the right document chunks, I focus on understanding the meaning behind both the question and the text. The system
looks at the actual concepts rather than just keywords. For example, if you ask about "a handsome person in Anupam's words," it knows to look for chunks containing 
terms like "সুপুরুষ" or "good-looking" in context.If a query is vague or missing details, the system first checks how confident it is in the matches. When confidence is low, it will ask you to clarify like
saying, "Do you mean Anupam’s description of a handsome man, or something else?" It also uses recent questions in the conversation to guess missing context (e.g., if you
previously mentioned "অনুপম," it assumes later questions might refer to him).

6.Do the results seem relevant? If not, what might improve them (e.g. better chunking, better embedding model, larger document)?
The results are usually good for direct facts (like ages, names, or events) but can struggle with open-ended questions (like "What’s the main theme?"). To improve, we could:
Better chunking: Split long paragraphs into clearer, idea-sized pieces.
A smarter model: Try newer Bengali or English embeddings.
More content: Add related books so the system has more reference points.
User feedback: Let users correct wrong answers to help the system learn.
Right now, it works well for textbook style answers but still needs tuning for deeper analysis. The biggest fixes would be sharper text splitting and more Bengali-specific training.


