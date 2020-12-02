{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Final Project - Word Cloud"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "For this project, you'll create a \"word cloud\" from a text by writing a script.  This script needs to process the text, remove punctuation, ignore case and words that do not contain all alphabets, count the frequencies, and ignore uninteresting or irrelevant words.  A dictionary is the output of the `calculate_frequencies` function.  The `wordcloud` module will then generate the image from your dictionary."
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "For the input text of your script, you will need to provide a file that contains text only.  For the text itself, you can copy and paste the contents of a website you like.  Or you can use a site like [Project Gutenberg](https://www.gutenberg.org/) to find books that are available online.  You could see what word clouds you can get from famous books, like a Shakespeare play or a novel by Jane Austen. Save this as a .txt file somewhere on your computer.\n",
    "<br><br>\n",
    "Now you will need to upload your input file here so that your script will be able to process it.  To do the upload, you will need an uploader widget.  Run the following cell to perform all the installs and imports for your word cloud script and uploader widget.  It may take a minute for all of this to run and there will be a lot of output messages. But, be patient. Once you get the following final line of output, the code is done executing. Then you can continue on with the rest of the instructions for this notebook.\n",
    "<br><br>\n",
    "**Enabling notebook extension fileupload/extension...**\n",
    "<br>\n",
    "**- Validating: <font color =green>OK</font>**"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 1,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Requirement already satisfied: wordcloud in /opt/conda/lib/python3.6/site-packages (1.8.0)\n",
      "Requirement already satisfied: matplotlib in /opt/conda/lib/python3.6/site-packages (from wordcloud) (3.0.3)\n",
      "Requirement already satisfied: numpy>=1.6.1 in /opt/conda/lib/python3.6/site-packages (from wordcloud) (1.15.4)\n",
      "Requirement already satisfied: pillow in /opt/conda/lib/python3.6/site-packages (from wordcloud) (5.4.1)\n",
      "Requirement already satisfied: cycler>=0.10 in /opt/conda/lib/python3.6/site-packages (from matplotlib->wordcloud) (0.10.0)\n",
      "Requirement already satisfied: kiwisolver>=1.0.1 in /opt/conda/lib/python3.6/site-packages (from matplotlib->wordcloud) (1.0.1)\n",
      "Requirement already satisfied: pyparsing!=2.0.4,!=2.1.2,!=2.1.6,>=2.0.1 in /opt/conda/lib/python3.6/site-packages (from matplotlib->wordcloud) (2.3.1)\n",
      "Requirement already satisfied: python-dateutil>=2.1 in /opt/conda/lib/python3.6/site-packages (from matplotlib->wordcloud) (2.8.0)\n",
      "Requirement already satisfied: six in /opt/conda/lib/python3.6/site-packages (from cycler>=0.10->matplotlib->wordcloud) (1.12.0)\n",
      "Requirement already satisfied: setuptools in /opt/conda/lib/python3.6/site-packages (from kiwisolver>=1.0.1->matplotlib->wordcloud) (40.8.0)\n",
      "Requirement already satisfied: fileupload in /opt/conda/lib/python3.6/site-packages (0.1.5)\n",
      "Requirement already satisfied: traitlets>=4.2 in /opt/conda/lib/python3.6/site-packages (from fileupload) (4.3.2)\n",
      "Requirement already satisfied: notebook>=4.2 in /opt/conda/lib/python3.6/site-packages (from fileupload) (5.7.5)\n",
      "Requirement already satisfied: ipywidgets>=5.1 in /opt/conda/lib/python3.6/site-packages (from fileupload) (7.4.2)\n",
      "Requirement already satisfied: ipython_genutils in /opt/conda/lib/python3.6/site-packages (from traitlets>=4.2->fileupload) (0.2.0)\n",
      "Requirement already satisfied: six in /opt/conda/lib/python3.6/site-packages (from traitlets>=4.2->fileupload) (1.12.0)\n",
      "Requirement already satisfied: decorator in /opt/conda/lib/python3.6/site-packages (from traitlets>=4.2->fileupload) (4.3.2)\n",
      "Requirement already satisfied: ipykernel in /opt/conda/lib/python3.6/site-packages (from notebook>=4.2->fileupload) (5.1.0)\n",
      "Requirement already satisfied: Send2Trash in /opt/conda/lib/python3.6/site-packages (from notebook>=4.2->fileupload) (1.5.0)\n",
      "Requirement already satisfied: tornado<7,>=4.1 in /opt/conda/lib/python3.6/site-packages (from notebook>=4.2->fileupload) (6.0.2)\n",
      "Requirement already satisfied: nbformat in /opt/conda/lib/python3.6/site-packages (from notebook>=4.2->fileupload) (4.4.0)\n",
      "Requirement already satisfied: pyzmq>=17 in /opt/conda/lib/python3.6/site-packages (from notebook>=4.2->fileupload) (18.0.1)\n",
      "Requirement already satisfied: jinja2 in /opt/conda/lib/python3.6/site-packages (from notebook>=4.2->fileupload) (2.10)\n",
      "Requirement already satisfied: jupyter-core>=4.4.0 in /opt/conda/lib/python3.6/site-packages (from notebook>=4.2->fileupload) (4.4.0)\n",
      "Requirement already satisfied: nbconvert in /opt/conda/lib/python3.6/site-packages (from notebook>=4.2->fileupload) (5.4.1)\n",
      "Requirement already satisfied: terminado>=0.8.1 in /opt/conda/lib/python3.6/site-packages (from notebook>=4.2->fileupload) (0.8.1)\n",
      "Requirement already satisfied: jupyter-client>=5.2.0 in /opt/conda/lib/python3.6/site-packages (from notebook>=4.2->fileupload) (5.2.4)\n",
      "Requirement already satisfied: prometheus-client in /opt/conda/lib/python3.6/site-packages (from notebook>=4.2->fileupload) (0.6.0)\n",
      "Requirement already satisfied: ipython>=4.0.0; python_version >= \"3.3\" in /opt/conda/lib/python3.6/site-packages (from ipywidgets>=5.1->fileupload) (7.4.0)\n",
      "Requirement already satisfied: widgetsnbextension~=3.4.0 in /opt/conda/lib/python3.6/site-packages (from ipywidgets>=5.1->fileupload) (3.4.2)\n",
      "Requirement already satisfied: jsonschema!=2.5.0,>=2.4 in /opt/conda/lib/python3.6/site-packages (from nbformat->notebook>=4.2->fileupload) (3.0.1)\n",
      "Requirement already satisfied: MarkupSafe>=0.23 in /opt/conda/lib/python3.6/site-packages (from jinja2->notebook>=4.2->fileupload) (1.1.1)\n",
      "Requirement already satisfied: mistune>=0.8.1 in /opt/conda/lib/python3.6/site-packages (from nbconvert->notebook>=4.2->fileupload) (0.8.4)\n",
      "Requirement already satisfied: pygments in /opt/conda/lib/python3.6/site-packages (from nbconvert->notebook>=4.2->fileupload) (2.3.1)\n",
      "Requirement already satisfied: entrypoints>=0.2.2 in /opt/conda/lib/python3.6/site-packages (from nbconvert->notebook>=4.2->fileupload) (0.3)\n",
      "Requirement already satisfied: bleach in /opt/conda/lib/python3.6/site-packages (from nbconvert->notebook>=4.2->fileupload) (3.1.0)\n",
      "Requirement already satisfied: pandocfilters>=1.4.1 in /opt/conda/lib/python3.6/site-packages (from nbconvert->notebook>=4.2->fileupload) (1.4.2)\n",
      "Requirement already satisfied: testpath in /opt/conda/lib/python3.6/site-packages (from nbconvert->notebook>=4.2->fileupload) (0.4.2)\n",
      "Requirement already satisfied: defusedxml in /opt/conda/lib/python3.6/site-packages (from nbconvert->notebook>=4.2->fileupload) (0.5.0)\n",
      "Requirement already satisfied: python-dateutil>=2.1 in /opt/conda/lib/python3.6/site-packages (from jupyter-client>=5.2.0->notebook>=4.2->fileupload) (2.8.0)\n",
      "Requirement already satisfied: setuptools>=18.5 in /opt/conda/lib/python3.6/site-packages (from ipython>=4.0.0; python_version >= \"3.3\"->ipywidgets>=5.1->fileupload) (40.8.0)\n",
      "Requirement already satisfied: jedi>=0.10 in /opt/conda/lib/python3.6/site-packages (from ipython>=4.0.0; python_version >= \"3.3\"->ipywidgets>=5.1->fileupload) (0.13.3)\n",
      "Requirement already satisfied: pickleshare in /opt/conda/lib/python3.6/site-packages (from ipython>=4.0.0; python_version >= \"3.3\"->ipywidgets>=5.1->fileupload) (0.7.5)\n",
      "Requirement already satisfied: prompt_toolkit<2.1.0,>=2.0.0 in /opt/conda/lib/python3.6/site-packages (from ipython>=4.0.0; python_version >= \"3.3\"->ipywidgets>=5.1->fileupload) (2.0.9)\n",
      "Requirement already satisfied: backcall in /opt/conda/lib/python3.6/site-packages (from ipython>=4.0.0; python_version >= \"3.3\"->ipywidgets>=5.1->fileupload) (0.1.0)\n",
      "Requirement already satisfied: pexpect in /opt/conda/lib/python3.6/site-packages (from ipython>=4.0.0; python_version >= \"3.3\"->ipywidgets>=5.1->fileupload) (4.6.0)\n",
      "Requirement already satisfied: attrs>=17.4.0 in /opt/conda/lib/python3.6/site-packages (from jsonschema!=2.5.0,>=2.4->nbformat->notebook>=4.2->fileupload) (19.1.0)\n",
      "Requirement already satisfied: pyrsistent>=0.14.0 in /opt/conda/lib/python3.6/site-packages (from jsonschema!=2.5.0,>=2.4->nbformat->notebook>=4.2->fileupload) (0.14.11)\n",
      "Requirement already satisfied: webencodings in /opt/conda/lib/python3.6/site-packages (from bleach->nbconvert->notebook>=4.2->fileupload) (0.5.1)\n",
      "Requirement already satisfied: parso>=0.3.0 in /opt/conda/lib/python3.6/site-packages (from jedi>=0.10->ipython>=4.0.0; python_version >= \"3.3\"->ipywidgets>=5.1->fileupload) (0.3.4)\n",
      "Requirement already satisfied: wcwidth in /opt/conda/lib/python3.6/site-packages (from prompt_toolkit<2.1.0,>=2.0.0->ipython>=4.0.0; python_version >= \"3.3\"->ipywidgets>=5.1->fileupload) (0.1.7)\n",
      "Requirement already satisfied: ptyprocess>=0.5 in /opt/conda/lib/python3.6/site-packages (from pexpect->ipython>=4.0.0; python_version >= \"3.3\"->ipywidgets>=5.1->fileupload) (0.6.0)\n",
      "Requirement already satisfied: ipywidgets in /opt/conda/lib/python3.6/site-packages (7.4.2)\n",
      "Requirement already satisfied: ipython>=4.0.0; python_version >= \"3.3\" in /opt/conda/lib/python3.6/site-packages (from ipywidgets) (7.4.0)\n",
      "Requirement already satisfied: widgetsnbextension~=3.4.0 in /opt/conda/lib/python3.6/site-packages (from ipywidgets) (3.4.2)\n",
      "Requirement already satisfied: nbformat>=4.2.0 in /opt/conda/lib/python3.6/site-packages (from ipywidgets) (4.4.0)\n",
      "Requirement already satisfied: traitlets>=4.3.1 in /opt/conda/lib/python3.6/site-packages (from ipywidgets) (4.3.2)\n",
      "Requirement already satisfied: ipykernel>=4.5.1 in /opt/conda/lib/python3.6/site-packages (from ipywidgets) (5.1.0)\n",
      "Requirement already satisfied: setuptools>=18.5 in /opt/conda/lib/python3.6/site-packages (from ipython>=4.0.0; python_version >= \"3.3\"->ipywidgets) (40.8.0)\n",
      "Requirement already satisfied: jedi>=0.10 in /opt/conda/lib/python3.6/site-packages (from ipython>=4.0.0; python_version >= \"3.3\"->ipywidgets) (0.13.3)\n",
      "Requirement already satisfied: decorator in /opt/conda/lib/python3.6/site-packages (from ipython>=4.0.0; python_version >= \"3.3\"->ipywidgets) (4.3.2)\n",
      "Requirement already satisfied: pickleshare in /opt/conda/lib/python3.6/site-packages (from ipython>=4.0.0; python_version >= \"3.3\"->ipywidgets) (0.7.5)\n",
      "Requirement already satisfied: prompt_toolkit<2.1.0,>=2.0.0 in /opt/conda/lib/python3.6/site-packages (from ipython>=4.0.0; python_version >= \"3.3\"->ipywidgets) (2.0.9)\n",
      "Requirement already satisfied: pygments in /opt/conda/lib/python3.6/site-packages (from ipython>=4.0.0; python_version >= \"3.3\"->ipywidgets) (2.3.1)\n",
      "Requirement already satisfied: backcall in /opt/conda/lib/python3.6/site-packages (from ipython>=4.0.0; python_version >= \"3.3\"->ipywidgets) (0.1.0)\n",
      "Requirement already satisfied: pexpect in /opt/conda/lib/python3.6/site-packages (from ipython>=4.0.0; python_version >= \"3.3\"->ipywidgets) (4.6.0)\n",
      "Requirement already satisfied: notebook>=4.4.1 in /opt/conda/lib/python3.6/site-packages (from widgetsnbextension~=3.4.0->ipywidgets) (5.7.5)\n",
      "Requirement already satisfied: ipython_genutils in /opt/conda/lib/python3.6/site-packages (from nbformat>=4.2.0->ipywidgets) (0.2.0)\n",
      "Requirement already satisfied: jsonschema!=2.5.0,>=2.4 in /opt/conda/lib/python3.6/site-packages (from nbformat>=4.2.0->ipywidgets) (3.0.1)\n",
      "Requirement already satisfied: jupyter_core in /opt/conda/lib/python3.6/site-packages (from nbformat>=4.2.0->ipywidgets) (4.4.0)\n",
      "Requirement already satisfied: six in /opt/conda/lib/python3.6/site-packages (from traitlets>=4.3.1->ipywidgets) (1.12.0)\n",
      "Requirement already satisfied: jupyter-client in /opt/conda/lib/python3.6/site-packages (from ipykernel>=4.5.1->ipywidgets) (5.2.4)\n",
      "Requirement already satisfied: tornado>=4.2 in /opt/conda/lib/python3.6/site-packages (from ipykernel>=4.5.1->ipywidgets) (6.0.2)\n",
      "Requirement already satisfied: parso>=0.3.0 in /opt/conda/lib/python3.6/site-packages (from jedi>=0.10->ipython>=4.0.0; python_version >= \"3.3\"->ipywidgets) (0.3.4)\n",
      "Requirement already satisfied: wcwidth in /opt/conda/lib/python3.6/site-packages (from prompt_toolkit<2.1.0,>=2.0.0->ipython>=4.0.0; python_version >= \"3.3\"->ipywidgets) (0.1.7)\n",
      "Requirement already satisfied: ptyprocess>=0.5 in /opt/conda/lib/python3.6/site-packages (from pexpect->ipython>=4.0.0; python_version >= \"3.3\"->ipywidgets) (0.6.0)\n",
      "Requirement already satisfied: pyzmq>=17 in /opt/conda/lib/python3.6/site-packages (from notebook>=4.4.1->widgetsnbextension~=3.4.0->ipywidgets) (18.0.1)\n",
      "Requirement already satisfied: Send2Trash in /opt/conda/lib/python3.6/site-packages (from notebook>=4.4.1->widgetsnbextension~=3.4.0->ipywidgets) (1.5.0)\n",
      "Requirement already satisfied: terminado>=0.8.1 in /opt/conda/lib/python3.6/site-packages (from notebook>=4.4.1->widgetsnbextension~=3.4.0->ipywidgets) (0.8.1)\n",
      "Requirement already satisfied: prometheus-client in /opt/conda/lib/python3.6/site-packages (from notebook>=4.4.1->widgetsnbextension~=3.4.0->ipywidgets) (0.6.0)\n",
      "Requirement already satisfied: jinja2 in /opt/conda/lib/python3.6/site-packages (from notebook>=4.4.1->widgetsnbextension~=3.4.0->ipywidgets) (2.10)\n",
      "Requirement already satisfied: nbconvert in /opt/conda/lib/python3.6/site-packages (from notebook>=4.4.1->widgetsnbextension~=3.4.0->ipywidgets) (5.4.1)\n",
      "Requirement already satisfied: attrs>=17.4.0 in /opt/conda/lib/python3.6/site-packages (from jsonschema!=2.5.0,>=2.4->nbformat>=4.2.0->ipywidgets) (19.1.0)\n",
      "Requirement already satisfied: pyrsistent>=0.14.0 in /opt/conda/lib/python3.6/site-packages (from jsonschema!=2.5.0,>=2.4->nbformat>=4.2.0->ipywidgets) (0.14.11)\n",
      "Requirement already satisfied: python-dateutil>=2.1 in /opt/conda/lib/python3.6/site-packages (from jupyter-client->ipykernel>=4.5.1->ipywidgets) (2.8.0)\n",
      "Requirement already satisfied: MarkupSafe>=0.23 in /opt/conda/lib/python3.6/site-packages (from jinja2->notebook>=4.4.1->widgetsnbextension~=3.4.0->ipywidgets) (1.1.1)\n",
      "Requirement already satisfied: mistune>=0.8.1 in /opt/conda/lib/python3.6/site-packages (from nbconvert->notebook>=4.4.1->widgetsnbextension~=3.4.0->ipywidgets) (0.8.4)\n",
      "Requirement already satisfied: entrypoints>=0.2.2 in /opt/conda/lib/python3.6/site-packages (from nbconvert->notebook>=4.4.1->widgetsnbextension~=3.4.0->ipywidgets) (0.3)\n",
      "Requirement already satisfied: bleach in /opt/conda/lib/python3.6/site-packages (from nbconvert->notebook>=4.4.1->widgetsnbextension~=3.4.0->ipywidgets) (3.1.0)\n",
      "Requirement already satisfied: pandocfilters>=1.4.1 in /opt/conda/lib/python3.6/site-packages (from nbconvert->notebook>=4.4.1->widgetsnbextension~=3.4.0->ipywidgets) (1.4.2)\n",
      "Requirement already satisfied: testpath in /opt/conda/lib/python3.6/site-packages (from nbconvert->notebook>=4.4.1->widgetsnbextension~=3.4.0->ipywidgets) (0.4.2)\n",
      "Requirement already satisfied: defusedxml in /opt/conda/lib/python3.6/site-packages (from nbconvert->notebook>=4.4.1->widgetsnbextension~=3.4.0->ipywidgets) (0.5.0)\n",
      "Requirement already satisfied: webencodings in /opt/conda/lib/python3.6/site-packages (from bleach->nbconvert->notebook>=4.4.1->widgetsnbextension~=3.4.0->ipywidgets) (0.5.1)\n",
      "Installing /opt/conda/lib/python3.6/site-packages/fileupload/static -> fileupload\n",
      "Up to date: /home/jovyan/.local/share/jupyter/nbextensions/fileupload/extension.js\n",
      "Up to date: /home/jovyan/.local/share/jupyter/nbextensions/fileupload/widget.js\n",
      "Up to date: /home/jovyan/.local/share/jupyter/nbextensions/fileupload/fileupload/widget.js\n",
      "- Validating: \u001b[32mOK\u001b[0m\n",
      "\n",
      "    To initialize this nbextension in the browser every time the notebook (or other app) loads:\n",
      "    \n",
      "          jupyter nbextension enable fileupload --user --py\n",
      "    \n",
      "Enabling notebook extension fileupload/extension...\n",
      "      - Validating: \u001b[32mOK\u001b[0m\n"
     ]
    }
   ],
   "source": [
    "# Here are all the installs and imports you will need for your word cloud script and uploader widget\n",
    "\n",
    "!pip install wordcloud\n",
    "!pip install fileupload\n",
    "!pip install ipywidgets\n",
    "!jupyter nbextension install --py --user fileupload\n",
    "!jupyter nbextension enable --py fileupload\n",
    "\n",
    "import wordcloud\n",
    "import numpy as np\n",
    "from matplotlib import pyplot as plt\n",
    "from IPython.display import display\n",
    "import fileupload\n",
    "import io\n",
    "import sys"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Whew! That was a lot. All of the installs and imports for your word cloud script and uploader widget have been completed. \n",
    "<br><br>\n",
    "**IMPORTANT!** If this was your first time running the above cell containing the installs and imports, you will need save this notebook now. Then under the File menu above,  select Close and Halt. When the notebook has completely shut down, reopen it. This is the only way the necessary changes will take affect.\n",
    "<br><br>\n",
    "To upload your text file, run the following cell that contains all the code for a custom uploader widget. Once you run this cell, a \"Browse\" button should appear below it. Click this button and navigate the window to locate your saved text file."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 23,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "application/vnd.jupyter.widget-view+json": {
       "model_id": "e7708ebd08cb48f3bad074f43eda134d",
       "version_major": 2,
       "version_minor": 0
      },
      "text/plain": [
       "FileUploadWidget(label='Browse', _dom_classes=('widget_item', 'btn-group'))"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    },
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Uploaded `New Text Document.txt` (0.24 kB)\n"
     ]
    }
   ],
   "source": [
    "# This is the uploader widget\n",
    "\n",
    "def _upload():\n",
    "\n",
    "    _upload_widget = fileupload.FileUploadWidget()\n",
    "\n",
    "    def _cb(change):\n",
    "        global file_contents\n",
    "        decoded = io.StringIO(change['owner'].data.decode('utf-8'))\n",
    "        filename = change['owner'].filename\n",
    "        print('Uploaded `{}` ({:.2f} kB)'.format(\n",
    "            filename, len(decoded.read()) / 2 **10))\n",
    "        file_contents = decoded.getvalue()\n",
    "\n",
    "    _upload_widget.observe(_cb, names='data')\n",
    "    display(_upload_widget)\n",
    "\n",
    "_upload()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "The uploader widget saved the contents of your uploaded file into a string object named *file_contents* that your word cloud script can process. This was a lot of preliminary work, but you are now ready to begin your script. "
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Write a function in the cell below that iterates through the words in *file_contents*, removes punctuation, and counts the frequency of each word.  Oh, and be sure to make it ignore word case, words that do not contain all alphabets and boring words like \"and\" or \"the\".  Then use it in the `generate_from_frequencies` function to generate your very own word cloud!\n",
    "<br><br>\n",
    "**Hint:** Try storing the results of your iteration in a dictionary before passing them into wordcloud via the `generate_from_frequencies` function."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 29,
   "metadata": {},
   "outputs": [],
   "source": [
    "def calculate_frequencies(file_contents):\n",
    "    # Here is a list of punctuations and uninteresting words you can use to process your text\n",
    "    punctuations = '''!()-[]{};:'\"\\,<>./?@#$%^&*_~'''\n",
    "    uninteresting_words = [\"the\", \"a\", \"to\", \"if\", \"is\", \"it\", \"of\", \"and\", \"or\", \"an\", \"as\", \"i\", \"me\", \"my\", \\\n",
    "    \"we\", \"our\", \"ours\", \"you\", \"your\", \"yours\", \"he\", \"she\", \"him\", \"his\", \"her\", \"hers\", \"its\", \"they\", \"them\", \\\n",
    "    \"their\", \"what\", \"which\", \"who\", \"whom\", \"this\", \"that\", \"am\", \"are\", \"was\", \"were\", \"be\", \"been\", \"being\", \\\n",
    "    \"have\", \"has\", \"had\", \"do\", \"does\", \"did\", \"but\", \"at\", \"by\", \"with\", \"from\", \"here\", \"when\", \"where\", \"how\", \\\n",
    "    \"all\", \"any\", \"both\", \"each\", \"few\", \"more\", \"some\", \"such\", \"no\", \"nor\", \"too\", \"very\", \"can\", \"will\", \"just\"]\n",
    "\n",
    "    # LEARNER CODE START HERE\n",
    "    non_punctuation_text=\"\"\n",
    "    for char in file_contents:\n",
    "        if char not in punctuations:\n",
    "            non_punctuation_text=non_punctuation_text+char\n",
    "    words=non_punctuation_text.split()\n",
    "    clean_words=[]\n",
    "    frequencies={}\n",
    "\n",
    "    for word in words:\n",
    "        if word.isalpha():\n",
    "            if word not in uninteresting_words:\n",
    "                clean_words.append(word)\n",
    "    for alpha_word in clean_words:\n",
    "        if alpha_word not in frequencies:\n",
    "            frequencies[alpha_word]=1\n",
    "        else:\n",
    "            frequencies[alpha_word]+=1\n",
    "    #wordcloud\n",
    "    cloud = wordcloud.WordCloud()\n",
    "    cloud.generate_from_frequencies(frequencies)\n",
    "    return cloud.to_array()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "If you have done everything correctly, your word cloud image should appear after running the cell below.  Fingers crossed!"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 31,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "# Display your wordcloud image\n",
    "myimage = calculate_frequencies(\"Humpty Dumpty is a character in an English nursery rhyme, probably originally a riddle and one of the best known in the English-speaking world. He is typically portrayed as an anthropomorphic egg, though he is not explicitly described as such. \")  \n",
    "plt.imshow(myimage, interpolation = 'nearest')\n",
    "plt.axis('off')\n",
    "plt.margins(x=0, y=0) \n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "If your word cloud image did not appear, go back and rework your `calculate_frequencies` function until you get the desired output.  Definitely check that you passed your frequecy count dictionary into the `generate_from_frequencies` function of `wordcloud`. Once you have correctly displayed your word cloud image, you are all done with this project. Nice work!"
   ]
  }
 ],
 "metadata": {
  "coursera": {
   "course_slug": "python-crash-course",
   "graded_item_id": "Z5d28",
   "launcher_item_id": "eSjyd"
  },
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.6.7"
  },
  "widgets": {
   "application/vnd.jupyter.widget-state+json": {
    "state": {},
    "version_major": 2,
    "version_minor": 0
   }
  }
 },
 "nbformat": 4,
 "nbformat_minor": 2
}
