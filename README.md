# Wordlist Creation Toolkit

![Wordlist Generation](https://via.placeholder.com/800x200.png?text=Wordlist+Generation+Toolkit)

A comprehensive guide and code collection for creating custom wordlists for security testing, password recovery, and linguistic research.

## Table of Contents
1. [Getting Started](#getting-started)
2. [Manual Collection](#manual-collection)
3. [Website Spidering](#cewl-spidering)
4. [Pattern Generation](#crunch)
5. [Word Mutation](#hashcat)
6. [Python Scripting](#python)
7. [Best Practices](#best-practices)
8. [Legal Disclaimer](#legal-disclaimer)

## <a name="getting-started"></a>🚀 Getting Started

### Prerequisites
- Linux environment (Kali Linux recommended)
- Basic command-line knowledge

### Installation
```bash
# Install required tools
sudo apt update
sudo apt install cewl crunch hashcat john python3

# Clone this repository
git clone https://github.com/blueiewu/wordlist-toolkit.git
cd wordlist-toolkit
```

## <a name="manual-collection"></a>📚 1. Manual Collection & Text Processing
Create wordlists from existing text sources

**Basic text processing:**
```bash
# Clean and normalize text
cat input.txt | tr 'A-Z' 'a-z' | tr -dc 'a-z0-9\n' | sort | uniq > cleaned.txt

# Filter by word length
awk 'length >= 8 && length <= 12' cleaned.txt > filtered.txt
```

## <a name="cewl-spidering"></a>🕷️ 2. Website Spidering with CeWL
Generate wordlists from website content

```bash
# Basic spidering (depth=2, min length=6)
cewl -d 2 -m 6 -w site_words.txt https://example.com

# With metadata extraction
cewl -d 3 -m 5 -e --meta_file meta.txt https://example.com
```

## <a name="crunch"></a>🔢 3. Pattern-Based Generation with Crunch
Create wordlists using character patterns

```bash
# Generate numeric PINs
crunch 4 4 0123456789 -o pins.txt

# Alphanumeric passwords
crunch 8 8 abcdefghijklmnopqrstuvwxyz0123456789 -o passwords.txt

# Pattern-based generation
crunch 8 8 -t p@ss%%% -o custom.txt  # Generates pass123, pass456, etc.
```

## <a name="hashcat"></a>🔄 4. Word Mutation with Hashcat
Modify existing wordlists

```bash
# Append numbers 00-99
hashcat -a 6 --stdout base_words.txt ?d?d > mutated.txt

# Apply leet-speak rules
hashcat -r /usr/share/hashcat/rules/leetspeak.rule --stdout base.txt > leet.txt

# Combine wordlists
hashcat -a 1 --stdout words1.txt words2.txt > combined.txt
```

## <a name="python"></a>🐍 5. Python Scripting
Custom wordlist generators

**wordlist_generator.py:**
```python
base_words = ["password", "summer", "admin", "winter"]
special_chars = ["!", "@", "#", "$"]

with open("custom_wordlist.txt", "w") as f:
    for word in base_words:
        # Year combinations
        for year in range(2000, 2025):
            f.write(f"{word}{year}\n")
        
        # Number suffixes
        for num in range(1, 100):
            f.write(f"{word}{num}\n")
        
        # Capitalization variants
        f.write(f"{word.capitalize()}\n")
        f.write(f"{word.upper()}\n")
        
        # Special character suffixes
        for char in special_chars:
            f.write(f"{word}{char}\n")
```

Run with:
```bash
python3 wordlist_generator.py
```

## <a name="best-practices"></a>🔐 6. Best Practices

### Optimization Tips
```bash
# Deduplicate wordlists
sort -u input.txt > deduped.txt

# Filter by length
awk 'length >= 8' input.txt > length8.txt

# Compress large files
gzip large_wordlist.txt
```

### Storage Requirements
| Character Set | Length | Approx. Size |
|---------------|--------|--------------|
| 0-9           | 8      | 100 MB       |
| a-z           | 6      | 300 MB       |
| a-z + 0-9     | 8      | 200 GB       |
| Full ASCII    | 8      | 70 TB        |

### Sample Workflow
1. Collect base words with CeWL from target website
2. Expand with Crunch using known patterns
3. Mutate with Hashcat rules (leet-speak, suffixes)
4. Filter: `sort -u | awk 'length >= 8' > final.txt`
5. Compress: `gzip final.txt`

## <a name="legal-disclaimer"></a>⚠️ 7. Legal Disclaimer

> **This toolkit is for educational and authorized security testing purposes only.**  
> ❌ Unauthorized use of these tools on systems you don't own is illegal.  
> ✅ Always obtain proper written authorization before performing security testing.  
> 🔒 Respect privacy and comply with all applicable laws and regulations.

---

**Contributing:**  
Pull requests are welcome! For major changes, please open an issue first to discuss proposed changes.

**License:**  
This project is licensed under the [MIT License](LICENSE).
