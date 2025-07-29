import streamlit as st
import subprocess
import tempfile
from Bio.Seq import Seq

st.set_page_config(page_title="GeneSmith: Text-to-Gene Compiler", layout="wide")
st.title("🧬 GeneSmith: Text-to-Gene from Natural Language")

# Step 1: Input - natural language description
nlp_input = st.text_area("🗣️ Describe the gene you want to build:",
                         "Express a fluorescent protein that binds zinc and localizes in mitochondria.",
                         height=150)

# Step 2: Parameters
col1, col2 = st.columns(2)
with col1:
    organism = st.selectbox("Select organism for codon optimization:", ["E. coli", "Human", "Yeast", "Custom"])
with col2:
    folding_check = st.checkbox("🔁 Predict RNA folding (via RNAfold)", value=True)

# Process button
if st.button("🧠 Compile Gene Sequence"):
    with st.spinner("Processing natural language and building gene..."):

        # Dummy NLP Mapping (mockup)
        protein = "MNGTEGPNFYVPFSNKTGVVR"  # Example amino acid string
        gene_seq = Seq(protein).back_transcribe()  # DNA (rough)
        rna_seq = gene_seq.transcribe()

        # Output area
        st.subheader("🔬 Results")
        st.markdown("**Generated DNA Sequence:**")
        st.code(str(gene_seq), language='text')

        st.markdown("**Transcribed RNA Sequence:**")
        st.code(str(rna_seq), language='text')

        if folding_check:
            try:
                with tempfile.NamedTemporaryFile(mode='w+', delete=False) as tmp:
                    tmp.write(str(rna_seq))
                    tmp.flush()
                    result = subprocess.run(['RNAfold', tmp.name], capture_output=True, text=True)

                output = result.stdout.strip().split("\n")
                if len(output) >= 2:
                    structure = output[1].split()[0]
                    mfe = output[1].split()[-1]
                    st.markdown("**RNA Folding Structure:**")
                    st.code(structure, language='text')
                    st.markdown(f"**Minimum Free Energy (MFE):** `{mfe}` kcal/mol")
                else:
                    st.warning("RNAfold did not return expected output.")
            except FileNotFoundError:
                st.error("RNAfold is not installed or not in PATH. Install ViennaRNA package to use folding prediction.")

    st.success("✅ Gene compiled and simulated successfully.")

st.markdown("---")
st.caption("Built by GeneSmith | Powered by Streamlit, BioPython & ViennaRNA")
