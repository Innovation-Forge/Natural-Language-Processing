# Text Analysis Script

## Introduction

The Text Analysis Script analyzes a collection of text files stored in a specified directory using the NLTK library. It tokenizes the text, calculates word frequencies, generates a concordance index to find word contexts, and provides detailed analysis of specific words. The results are displayed in a neatly formatted table using PrettyTable.

## Features

- **Text Tokenization**: Breaks down text into words and punctuation.
- **Word Frequency Analysis**: Calculates the frequency of each word in the text.
- **Concordance Index**: Generates an index to find word contexts in the text.
- **Detailed Word Analysis**: Analyzes specific words for occurrences and similar contexts.
- **Formatted Output**: Displays results in a neatly formatted table using PrettyTable.

## Prerequisites

- Python 3.6 or higher.
- Required Python libraries: `nltk`, `prettytable`, `textwrap`, `io`, `sys`.

## Modules

- **nltk**: Provides tools for working with human language data.
- **prettytable**: Creates ASCII tables for displaying data.
- **textwrap**: Wraps text to a specified width.
- **io**: Handles I/O operations.
- **sys**: Provides system-specific parameters and functions.

```python
import nltk
from nltk import FreqDist, ConcordanceIndex
from nltk.corpus.reader.plaintext import PlaintextCorpusReader
from prettytable import PrettyTable
import textwrap
import io
import sys
```

## Functions

### `load_corpus(corpus_root)`

Loads the text files from the specified directory and interprets them as a corpus.

- **Parameters:**
  - `corpus_root` (str): The path to the directory containing text files.
- **Returns:**
  - `PlaintextCorpusReader`: The loaded corpus.

#### Example

```python
def load_corpus(corpus_root):
    return PlaintextCorpusReader(corpus_root, '.*\.txt')
```

### `analyze_corpus(corpus)`

Performs initial analysis on the corpus, including tokenization, frequency distribution, and concordance indexing.

- **Parameters:**
  - `corpus` (`PlaintextCorpusReader`): The loaded corpus.
- **Returns:**
  - `nltk.Text`: The text object for further analysis.
  - `FreqDist`: The frequency distribution of words.
  - `ConcordanceIndex`: The concordance index for word contexts.

#### Example

```python
def analyze_corpus(corpus):
    tokens = corpus.words()
    fdist = FreqDist(tokens)
    text = nltk.Text(tokens)
    concord_index = ConcordanceIndex(text.tokens)
    
    print("\nCorpus Statistics:")
    print("-------------------")
    print(f"Corpus Length: {len(text)}")
    print(f"Number of Word Tokens: {len(tokens)}")
    print(f"Size of Vocabulary: {len(set(tokens))}\n")
    
    return text, fdist, concord_index
```

### `wrap_text(text, width=80)`

Formats long strings of text to fit within a specified width on screen.

- **Parameters:**
  - `text` (str): The text to wrap.
  - `width` (int): The width to wrap the text to.
- **Returns:**
  - `str`: The wrapped text.

#### Example

```python
def wrap_text(text, width=80):
    return "\n".join(textwrap.wrap(text, width))
```

### `capture_similar_words(text, word)`

Captures the output of NLTK's `similar()` method, which is usually printed directly to the console.

- **Parameters:**
  - `text` (`nltk.Text`): The text object for analysis.
  - `word` (str): The word to find similar contexts for.
- **Returns:**
  - `str`: The captured output of similar words.

#### Example

```python
def capture_similar_words(text, word):
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    text.similar(word)
    output = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return output.strip()
```

### `print_detailed_analysis(text, fdist, concord_index)`

Prints detailed analysis of specific words in the corpus, including occurrences and similar words.

- **Parameters:**
  - `text` (`nltk.Text`): The text object for analysis.
  - `fdist` (`FreqDist`): The frequency distribution of words.
  - `concord_index` (`ConcordanceIndex`): The concordance index for word contexts.
- **Returns:**
  - `None`

#### Example

```python
def print_detailed_analysis(text, fdist, concord_index):
    test_words = ['GLOVE', 'GUN', 'BRONCO', 'BLOOD', 'GUILTY']
    table = PrettyTable(["Word", "Occurrences", "Similar Words"])
    for word in test_words:
        occurrence = fdist[word.upper()]
        similar_words = capture_similar_words(text, word)
        table.add_row([word, occurrence, similar_words])
    
    print("Word Details:")
    print("-------------")
    print(table)
    
    for word in test_words:
        print(f"\nConcordances for '{word}':")
        concordance_lines = text.concordance_list(word, width=80, lines=len(text.tokens))
        for line in concordance_lines:
            print(wrap_text(line.line))
```

## Main Function

### `main()`

The main function orchestrates loading the corpus, analyzing it, and printing the analysis.

- **Returns:**
  - `None`

#### Example

```python
def main():
    corpus_root = 'C:\\Users\\james\\CORPUS'
    corpus = load_corpus(corpus_root)
    text, fdist, concord_index = analyze_corpus(corpus)
    print_detailed_analysis(text, fdist, concord_index)

if __name__ == "__main__":
    main()
```

## Usage

1. **Install NLTK and PrettyTable**: Ensure the required libraries are installed.

    ```bash
    pip install nltk prettytable
    ```

2. **Prepare the Corpus**: Place your text files in a directory (e.g., `C:\\Users\\james\\CORPUS`).

3. **Run the Script**: Execute the script in your Python environment.

    ```bash
    python script_name.py
    ```

4. **View the Output**: The script will display corpus statistics, detailed word analysis, and word concordances.

## Example Output

```plaintext
Corpus Statistics:
-------------------
Corpus Length: 12345
Number of Word Tokens: 67890
Size of Vocabulary: 2345

Word Details:
-------------
+-------+-------------+------------------------------------------+
| Word  | Occurrences | Similar Words                            |
+-------+-------------+------------------------------------------+
| GLOVE | 10          | hand, finger, ...                        |
| GUN   | 20          | weapon, pistol, ...                      |
| BRONCO| 15          | vehicle, car, ...                        |
| BLOOD | 25          | red, liquid, ...                         |
| GUILTY| 30          | innocent, blameworthy, ...               |
+-------+-------------+------------------------------------------+

Concordances for 'GLOVE':
<concordance lines>
```

## Error Handling

- **File Errors**: Ensure the specified directory and text files are accessible.
- **NLTK Resource Download**: The script automatically downloads essential NLTK resources.

## Security Considerations

- **Data Privacy**: Ensure the text data does not contain sensitive information if shared or analyzed publicly.

## FAQs

**Q: What text file formats are supported?**
A: The script supports plain text files (.txt).

**Q: Can I use my own words for detailed analysis?**
A: Yes, modify the `test_words` list in the `print_detailed_analysis` function.

**Q: How can I change the corpus directory?**
A: Update the `corpus_root` variable in the `main()` function to your desired directory.

## Troubleshooting

- **Module Not Found Errors**: Ensure Python is installed correctly and all necessary modules are available. Install any missing modules using `pip`.
- **File Not Found**: Ensure the text files are in the specified directory and the path is correct.

For further assistance or to report bugs, please reach out to the repository maintainers or open an issue on the project's issue tracker.
