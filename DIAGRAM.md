%%{init: {'theme': 'forest', 'themeVariables': { 'primaryColor': '#0A1E35', 'edgeLabelBackground':'#152D4F', 'tertiaryColor': '#fff'}}}%%
graph TD
    %% Define Styles
    classDef file fill:#E1F5FE,stroke:#01579B,stroke-width:2px;
    classDef process fill:#C8E6C9,stroke:#1B5E20,stroke-width:2px;
    classDef database fill:#FFF9C4,stroke:#FBC02D,stroke-width:2px;
    classDef ai fill:#E1BEE7,stroke:#4A148C,stroke-width:2px;
    classDef user fill:#FFCCBC,stroke:#E64A19,stroke-width:2px;

    subgraph DataIn ["A: Data Ingestion (Local Knowledge Base)"]
        PDF[("📄 NT Power 2026 Tariff (EB-2025-0021)")]:::file
        PyPDF[("🛠️ PyPDF Parser")]:::process
    end

    subgraph Embedding ["B: Modern Embedding Pipeline"]
        TextSplitter[("✂️ RecursiveCharacterTextSplitter (Chunk=500)")]:::process
        GeminiEmbeddings[("🔮 gemini-embedding-001 (Google)")]:::ai
        ChromaDB[("🗄️ ChromaDB Vector Store (Local DB)")]:::database
    end

    subgraph RAGChain ["C: Retrieval-Augmented Generation (Q&A)"]
        UserQuestion("❓ Resident Question"):::user
        LangChainClassic[("🔗 RetrievalQA Chain (LangChain-Classic)")]:::process
        GeminiFlash[("🧠 Gemini 1.5 Flash (LLM Brain)")]:::ai
        FriendlyAnswer("✅ 2026 Energy Advice"):::user
    end

    %% Diagram Flows (Connections)
    PDF --> PyPDF
    PyPDF --> TextSplitter
    TextSplitter --> GeminiEmbeddings
    GeminiEmbeddings --> ChromaDB
    UserQuestion --> LangChainClassic
    LangChainClassic -- "1. Semantic Search (Cheapest Time?)" --> ChromaDB
    ChromaDB -- "2. Retrieve Relevant Chunk (3.9¢ ULO)" --> LangChainClassic
    LangChainClassic -- "3. Prompt + Context" --> GeminiFlash
    GeminiFlash -- "4. Human-Readable Answer" --> FriendlyAnswer
