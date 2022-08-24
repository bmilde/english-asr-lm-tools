# english-asr-lm-tools

These tools can be used to prepare texts for asr tasks.
A toolset for german can be found: https://github.com/bmilde/german-asr-lm-tools/

These tools work best with [the Pile](https://pile.eleuther.ai/) dataset and the officals [wikipedia dumps](https://dumps.wikimedia.org/enwiki/latest/).

## Requirements
At least one (or more!) terabyte of free space because the Pile is decompressed 825GB large and some of the following steps create copys of the dataset.
Due to the multiprocessing plenty of RAM and CPUs are also recommended.

ZST library to decompress the Pile
```
sudo apt install zstd
```
Create and start a virtual environment
```
virtualenv -p python3.9 env
source env/bin/active
pip install -r requirements.txt
python -m spacy download en_core_web_sm
```
## Download and decompress Files
At first download and decompress the Pile. This dataset is used for the Youtube Subtitles:
```
./downloadPile.sh
```
The 30 parts are now decompressed in the `pile` folder.

To process wikipedia articles the latest dump is used:
```
wget https://dumps.wikimedia.org/enwiki/latest/enwiki-latest-pages-articles-multistream.xml.bz2
```

## Sort the files
To run the scripts we need to unmix the files at first.
in `convertPile.py` in Line 10 you can define the needed datasources.
The unmixed data is stored into the pileSorted folder.
To sort the files start
```
python convertPile.py
```
## Youtube Subtitles
To prepare Youtube Subtitles run the script:
```
python ytcleanup.py -o ytClean -n 30
```
`-o` sets the output folder (eg ytClean), `-n` sets the number of processes and `-p` preserves punctuation if needed.
The resulting files can be concatenated to one file.
```
cat ytClean/* > YouTubeClean
```
