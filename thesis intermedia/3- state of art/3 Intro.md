# Estrutura resumida

| Etapa                     | Exemplos de tecnologia          | Vantagens         | Limitações                   | Aderência ao teu projeto        |
| ------------------------- | ------------------------------- | ----------------- | ---------------------------- | ------------------------------- |
| 1. Prompt/Intent          | Llama 3/Mistral; LangChain      | NL livre; diálogo | Ambiguidade; audit trail     | Interface conversacional pedida |
| 2. Semantic Mapping       | dbt/Cube/LookML                 | Consistência KPIs | Manutenção                   | “Semantically aligned JSON”     |
| 3. JSON Generation        | Function Calling; Guardrails    | Saída estruturada | JSON inválido sem restrições | “Valid JSON configuration”      |
| 4. Validation & Rendering | JSON Schema; renderer existente | Execução imediata | Só sintaxe ≠ semântica       | Integração com motor existente  |
| 5. Refinement Loop        | ReAct; JSON patch               | Iteração rápida   | Gestão de estado             | Feedback loop explícito         |
|                           |                                 |                   |                              |                                 |


1. **Usar um que tenha uma API publica** 

| Modelo                     | Plataforma/API                                                                                                | Vantagens                                           | Limitações                               |
| -------------------------- | ------------------------------------------------------------------------------------------------------------- | --------------------------------------------------- | ---------------------------------------- |
| **Mistral 7B**             | [Hugging Face Inference API](https://huggingface.co/mistralai/Mistral-7B-Instruct-v0.2) ou local              | Gratuito; muito rápido; excelente para JSON         | Menor compreensão de contexto longo      |
| **Llama 3 (Meta)**         | [Hugging Face](https://huggingface.co/meta-llama/Meta-Llama-3-8B-Instruct) ou [Groq Cloud](https://groq.com/) | Gratuito; boa qualidade; resposta quase instantânea | Pior estruturação de texto que GPT-4     |
| **Falcon 7B/40B**          | [Hugging Face](https://huggingface.co/tiiuae/falcon-7b-instruct)                                              | Open-source, suporta fine-tuning                    | Menor precisão sem instruções detalhadas |
| **Gemma 2 (Google)**       | [Kaggle Models / Hugging Face](https://huggingface.co/google/gemma-2-9b)                                      | Nova geração, boa consistência                      | Precisa GPU potente se correres local    |
| **Mixtral (Mistral 8x7B)** | [Hugging Face](https://huggingface.co/mistralai/Mixtral-8x7B-Instruct-v0.1)                                   | Muito bom para JSON e raciocínio lógico             | Modelo pesado (~80GB se local)           |
Usar o **Groq Cloud com Llama 3 (8B)** — é gratuito, extremamente rápido e **funciona com o mesmo código da OpenAI API**.

2.  **Vantagens e desvantagens de cada tecnologia**

| Tecnologia                          | Vantagens                                                          | Desvantagens                                                          |
| ----------------------------------- | ------------------------------------------------------------------ | --------------------------------------------------------------------- |
| **dbt**                             | Open-source, documenta métricas, garante consistência no warehouse | Foco em transformação, não em visualização; requer infraestrutura SQL |
| **Cube.js**                         | Excelente API semântica, suporta tempo real, ideal para dashboards | Mais complexo de escalar; requer configuração em Node.js              |
| **LookML**                          | Linguagem madura, suporte empresarial, forte integração com Looker | Proprietário (Google Cloud), menos flexível fora do Looker            |
| **Conceito Semântico (o teu caso)** | Pode ser leve e adaptado ao domínio CCaaS, integrado no backend    | Tens de definir manualmente as métricas e regras no teu modelo        |
3. **Function Calling** 
- Obriga o modelo a **responder com dados estruturados** (JSON, parâmetros, etc.).
    
- Permite ao LLM **“chamar” funções reais** no teu sistema (ex.: `create_dashboard()`, `update_metric()`).
    
- Melhora muito a **fiabilidade** e reduz _hallucinations_.

**Guardrails**
**Guardrails AI** é uma biblioteca criada para **validar, corrigir e proteger as respostas de LLMs** — especialmente quando precisas de JSON correto e coerente.
Podes pensar nela como um **“filtro de segurança”** que vem **depois do LLM** e antes do teu sistema usar a resposta.







