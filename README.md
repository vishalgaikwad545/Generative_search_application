# Comprehensive Documentation for Generative Search Application

## Project Overview

### Project Goals
The goal of this project is to build a robust Generative Search System capable of effectively and accurately answering questions from various policy documents. This application is designed to assist users in navigating and extracting specific information from lengthy documents, particularly insurance policy documents, by using natural language queries.

### Target Audience
The application is intended for users who need to query large volumes of text, such as policy documents, legal agreements, or academic papers, and retrieve relevant information quickly and accurately. It is particularly useful in the insurance industry, where policy documents are often complex and lengthy.

## Data Sources

### Document Input
- **PDF Documents**: Users upload PDF files containing policy documents or any other relevant text-based data. The content of these PDFs is extracted and processed for further use in the application.

## Design Choices

### Technology Stack
- **Streamlit**: Streamlit is used to create a user-friendly web interface for the application, allowing users to interact with the system, upload documents, and ask questions.
- **LangChain**: LangChain provides the framework to manage and orchestrate the various components needed for the generative search application, including text splitting, vector store management, and model interfacing.
- **Google Generative AI**: The project leverages Google's Generative AI models for embedding generation and conversational AI. This includes both embeddings for vector search and a generative model for answering questions.
- **FAISS (Facebook AI Similarity Search)**: FAISS is employed as the vector store to perform similarity searches efficiently on the text chunks generated from the PDF content.

### Components

1. **Text Extraction**:
   - The application uses `PyPDF2` to extract text from the uploaded PDF documents.

2. **Text Chunking**:
   - Given the large size of documents, the text is split into manageable chunks using `RecursiveCharacterTextSplitter` to ensure efficient processing and accurate results.

3. **Vector Store**:
   - The text chunks are converted into embeddings using the Google Generative AI embedding model. These embeddings are then stored in a FAISS index, allowing for fast similarity searches when querying the documents.

4. **Conversational Chain**:
   - A custom prompt is used to interact with Google's Generative AI model (specifically `gemini-pro`), ensuring that the model provides accurate and contextually relevant answers. The system is designed to refrain from providing incorrect answers when the context is insufficient.

### User Interface
- **Streamlit Interface**:
  - The application interface is built using Streamlit, providing a simple and intuitive experience for the user. The main features include:
    - A field to input the Google API key.
    - A file uploader for PDF documents.
    - A text input for submitting questions.
    - A sidebar for navigating the application features.

### Challenges Faced

1. **Text Extraction Accuracy**:
   - Extracting text from PDFs can be error-prone due to the varied formatting and structure of documents. Ensuring consistent and clean text extraction was essential for maintaining the accuracy of the generative search.

2. **Chunking Large Documents**:
   - Deciding on the chunk size was challenging because it required balancing the need to capture complete thoughts and contexts with the limitations on input size for embeddings and model processing.

3. **API Key Management**:
   - Securing and managing the Google API key is critical, especially when allowing users to input their keys. Ensuring the key is only used in secure, authenticated environments was a priority.

4. **Generating Accurate Responses**:
   - Ensuring the AI model provides accurate and contextually appropriate responses required careful tuning of the prompt and temperature settings in the `ChatGoogleGenerativeAI` model.

### Future Improvements

1. **Enhanced Error Handling**:
   - Implement more robust error handling, especially for edge cases like corrupt PDFs, incorrect API keys, or excessively large documents that might cause processing delays or failures.

2. **User Authentication**:
   - Implement user authentication to securely manage API keys and provide personalized settings and document histories.

3. **Multi-Document Querying**:
   - Enable the ability to query across multiple documents simultaneously, improving the utility of the application for users dealing with large volumes of data.

4. **Expanded Model Support**:
   - Integrate additional models or allow users to choose from a selection of generative models to tailor the application to different domains or query types.

## Code Walkthrough

### Streamlit Application Setup
- **Page Configuration**:
  - The application page is configured with a wide layout and a title "Document Genie" using `st.set_page_config`.
  
- **Markdown Instructions**:
  - The initial instructions on how to use the application are provided through a `st.markdown` block, guiding users on entering the API key, uploading documents, and asking questions.

### Core Functions

1. **`get_pdf_text(pdf_docs)`**:
   - This function loops through the uploaded PDF files and extracts the text from each page using `PyPDF2`. The extracted text is concatenated and returned as a single string.

2. **`get_text_chunks(text)`**:
   - Takes the full text of the document and splits it into smaller chunks. This is necessary for efficient embedding generation and vector search. `RecursiveCharacterTextSplitter` is used to ensure chunks are of appropriate size and contextually coherent.

3. **`get_vector_store(text_chunks, api_key)`**:
   - Converts the text chunks into embeddings using Google Generative AI and stores them in a FAISS index, which is then saved locally for later retrieval.

4. **`get_conversational_chain()`**:
   - Sets up the QA chain using a predefined prompt and a generative model. This chain is used to generate responses to user queries based on the context retrieved from the vector store.

5. **`user_input(user_question, api_key)`**:
   - Retrieves relevant document chunks from the FAISS index using a similarity search based on the user’s question. The retrieved chunks are then fed into the QA chain, and the generated response is displayed to the user.

### Main Application Logic
- **`main()`**:
  - This is the entry point of the application. It handles the user interface logic, including displaying the text input for the user's question, handling file uploads, and processing the documents to generate and store embeddings.

### Execution
- **`if __name__ == "__main__":`**:
  - Ensures that the `main()` function is called only when the script is run directly, not when it is imported as a module.

---

This documentation provides a clear understanding of the project, from the initial goals and design choices to the specific challenges encountered and the future improvements planned.
