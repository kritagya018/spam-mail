



Spam Mail Prediction using Machine Learning
This repository offers a comprehensive solution for efficiently identifying and filtering spam emails using machine learning. By incorporating data analysis, preprocessing techniques, and model development, this project serves as a guide to creating a robust spam mail prediction system. It is designed to enhance email security and reduce unwanted inbox clutter.

Project Structure
Data Loading: The project begins with the loading of data, a crucial step in understanding and preparing the dataset for further analysis.
Data Cleansing and Preprocessing: The dataset undergoes thorough cleaning and preprocessing to ensure its readiness for subsequent analysis and model development.
Data Preparation for Classification Models: Following preprocessing, the data is prepared specifically for training and evaluating classification models.
Model Building and Training: The project involves building various machine learning models tailored for spam mail prediction. These models are then trained using the prepared dataset.
Model Evaluation: The performance of each model is evaluated to determine its effectiveness in identifying and filtering spam emails.
Problem Approach
While primarily addressing the spam mail prediction as a classification problem, this project encompasses data-driven strategies for efficient model development.

System Requirements
No specific prerequisites are required for system compatibility. All you need is a properly functioning PC and an internet connection. The project is developed in Google Colab for ease of use. Simply load the provided files in Google Colab, and you're ready to initiate the development of an effective spam mail prediction system.

About
Efficiently identify and filter spam emails with this machine learning project. Through data analysis, preprocessing, and model development, this repository guides you in creating a robust spam mail prediction system. Perfect for enhancing email security and reducing unwanted inbox clutter.























{
  "nbformat": 4,
  "nbformat_minor": 0,
  "metadata": {
    "colab": {
      "provenance": [],
      "authorship_tag": "ABX9TyPmq8xMFAEMjSU7uVocIHvU",
      "include_colab_link": true
    },
    "kernelspec": {
      "name": "python3",
      "display_name": "Python 3"
    },
    "language_info": {
      "name": "python"
    }
  },
  "cells": [
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "view-in-github",
        "colab_type": "text"
      },
      "source": [
        "<a href=\"https://colab.research.google.com/github/Tukesh1/spam-mail-predection-using-ML/blob/main/spam_Mail_Predection_using_Machine_Learning_.ipynb\" target=\"_parent\"><img src=\"https://colab.research.google.com/assets/colab-badge.svg\" alt=\"Open In Colab\"/></a>"
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "kqB21QOgMg-G"
      },
      "source": [
        "Importing the Dependencies"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "rALI06-oHusw"
      },
      "source": [
        "import numpy as np\n",
        "import pandas as pd\n",
        "from sklearn.model_selection import train_test_split\n",
        "from sklearn.feature_extraction.text import TfidfVectorizer\n",
        "from sklearn.linear_model import LogisticRegression\n",
        "from sklearn.metrics import accuracy_score"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "YyKe9o2ONeFv"
      },
      "source": [
        "Data Collection & Pre-Processing"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "CpStHH8KNcYB"
      },
      "source": [
        "# loading the data from csv file to a pandas Dataframe\n",
        "raw_mail_data = pd.read_csv('/content/mail_data.csv')"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "pdn-7VE2NxsZ",
        "outputId": "bbc52380-c865-4cc7-f38f-edb549cd298c"
      },
      "source": [
        "print(raw_mail_data)"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "     Category                                            Message\n",
            "0         ham  Go until jurong point, crazy.. Available only ...\n",
            "1         ham                      Ok lar... Joking wif u oni...\n",
            "2        spam  Free entry in 2 a wkly comp to win FA Cup fina...\n",
            "3         ham  U dun say so early hor... U c already then say...\n",
            "4         ham  Nah I don't think he goes to usf, he lives aro...\n",
            "...       ...                                                ...\n",
            "5567     spam  This is the 2nd time we have tried 2 contact u...\n",
            "5568      ham               Will ü b going to esplanade fr home?\n",
            "5569      ham  Pity, * was in mood for that. So...any other s...\n",
            "5570      ham  The guy did some bitching but I acted like i'd...\n",
            "5571      ham                         Rofl. Its true to its name\n",
            "\n",
            "[5572 rows x 2 columns]\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "yhakjIE1N011"
      },
      "source": [
        "# replace the null values with a null string\n",
        "mail_data = raw_mail_data.where((pd.notnull(raw_mail_data)),'')"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 206
        },
        "id": "SJey6H-SOWeK",
        "outputId": "0be08ea8-5b8b-40f3-89bc-2b6984d7b8ea"
      },
      "source": [
        "# printing the first 5 rows of the dataframe\n",
        "mail_data.head()"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "  Category                                            Message\n",
              "0      ham  Go until jurong point, crazy.. Available only ...\n",
              "1      ham                      Ok lar... Joking wif u oni...\n",
              "2     spam  Free entry in 2 a wkly comp to win FA Cup fina...\n",
              "3      ham  U dun say so early hor... U c already then say...\n",
              "4      ham  Nah I don't think he goes to usf, he lives aro..."
            ],
            "text/html": [
              "\n",
              "  <div id=\"df-953b7838-70d8-4bf0-b9e4-d4e70f9032aa\" class=\"colab-df-container\">\n",
              "    <div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>Category</th>\n",
              "      <th>Message</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>ham</td>\n",
              "      <td>Go until jurong point, crazy.. Available only ...</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>ham</td>\n",
              "      <td>Ok lar... Joking wif u oni...</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>spam</td>\n",
              "      <td>Free entry in 2 a wkly comp to win FA Cup fina...</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>ham</td>\n",
              "      <td>U dun say so early hor... U c already then say...</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>ham</td>\n",
              "      <td>Nah I don't think he goes to usf, he lives aro...</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>\n",
              "    <div class=\"colab-df-buttons\">\n",
              "\n",
              "  <div class=\"colab-df-container\">\n",
              "    <button class=\"colab-df-convert\" onclick=\"convertToInteractive('df-953b7838-70d8-4bf0-b9e4-d4e70f9032aa')\"\n",
              "            title=\"Convert this dataframe to an interactive table.\"\n",
              "            style=\"display:none;\">\n",
              "\n",
              "  <svg xmlns=\"http://www.w3.org/2000/svg\" height=\"24px\" viewBox=\"0 -960 960 960\">\n",
              "    <path d=\"M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z\"/>\n",
              "  </svg>\n",
              "    </button>\n",
              "\n",
              "  <style>\n",
              "    .colab-df-container {\n",
              "      display:flex;\n",
              "      gap: 12px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert {\n",
              "      background-color: #E8F0FE;\n",
              "      border: none;\n",
              "      border-radius: 50%;\n",
              "      cursor: pointer;\n",
              "      display: none;\n",
              "      fill: #1967D2;\n",
              "      height: 32px;\n",
              "      padding: 0 0 0 0;\n",
              "      width: 32px;\n",
              "    }\n",
              "\n",
              "    .colab-df-convert:hover {\n",
              "      background-color: #E2EBFA;\n",
              "      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);\n",
              "      fill: #174EA6;\n",
              "    }\n",
              "\n",
              "    .colab-df-buttons div {\n",
              "      margin-bottom: 4px;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert {\n",
              "      background-color: #3B4455;\n",
              "      fill: #D2E3FC;\n",
              "    }\n",
              "\n",
              "    [theme=dark] .colab-df-convert:hover {\n",
              "      background-color: #434B5C;\n",
              "      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);\n",
              "      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));\n",
              "      fill: #FFFFFF;\n",
              "    }\n",
              "  </style>\n",
              "\n",
              "    <script>\n",
              "      const buttonEl =\n",
              "        document.querySelector('#df-953b7838-70d8-4bf0-b9e4-d4e70f9032aa button.colab-df-convert');\n",
              "      buttonEl.style.display =\n",
              "        google.colab.kernel.accessAllowed ? 'block' : 'none';\n",
              "\n",
              "      async function convertToInteractive(key) {\n",
              "        const element = document.querySelector('#df-953b7838-70d8-4bf0-b9e4-d4e70f9032aa');\n",
              "        const dataTable =\n",
              "          await google.colab.kernel.invokeFunction('convertToInteractive',\n",
              "                                                    [key], {});\n",
              "        if (!dataTable) return;\n",
              "\n",
              "        const docLinkHtml = 'Like what you see? Visit the ' +\n",
              "          '<a target=\"_blank\" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'\n",
              "          + ' to learn more about interactive tables.';\n",
              "        element.innerHTML = '';\n",
              "        dataTable['output_type'] = 'display_data';\n",
              "        await google.colab.output.renderOutput(dataTable, element);\n",
              "        const docLink = document.createElement('div');\n",
              "        docLink.innerHTML = docLinkHtml;\n",
              "        element.appendChild(docLink);\n",
              "      }\n",
              "    </script>\n",
              "  </div>\n",
              "\n",
              "\n",
              "<div id=\"df-2c099faf-dfe2-48a8-8cc6-bddf16fa8e6e\">\n",
              "  <button class=\"colab-df-quickchart\" onclick=\"quickchart('df-2c099faf-dfe2-48a8-8cc6-bddf16fa8e6e')\"\n",
              "            title=\"Suggest charts\"\n",
              "            style=\"display:none;\">\n",
              "\n",
              "<svg xmlns=\"http://www.w3.org/2000/svg\" height=\"24px\"viewBox=\"0 0 24 24\"\n",
              "     width=\"24px\">\n",
              "    <g>\n",
              "        <path d=\"M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z\"/>\n",
              "    </g>\n",
              "</svg>\n",
              "  </button>\n",
              "\n",
              "<style>\n",
              "  .colab-df-quickchart {\n",
              "      --bg-color: #E8F0FE;\n",
              "      --fill-color: #1967D2;\n",
              "      --hover-bg-color: #E2EBFA;\n",
              "      --hover-fill-color: #174EA6;\n",
              "      --disabled-fill-color: #AAA;\n",
              "      --disabled-bg-color: #DDD;\n",
              "  }\n",
              "\n",
              "  [theme=dark] .colab-df-quickchart {\n",
              "      --bg-color: #3B4455;\n",
              "      --fill-color: #D2E3FC;\n",
              "      --hover-bg-color: #434B5C;\n",
              "      --hover-fill-color: #FFFFFF;\n",
              "      --disabled-bg-color: #3B4455;\n",
              "      --disabled-fill-color: #666;\n",
              "  }\n",
              "\n",
              "  .colab-df-quickchart {\n",
              "    background-color: var(--bg-color);\n",
              "    border: none;\n",
              "    border-radius: 50%;\n",
              "    cursor: pointer;\n",
              "    display: none;\n",
              "    fill: var(--fill-color);\n",
              "    height: 32px;\n",
              "    padding: 0;\n",
              "    width: 32px;\n",
              "  }\n",
              "\n",
              "  .colab-df-quickchart:hover {\n",
              "    background-color: var(--hover-bg-color);\n",
              "    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);\n",
              "    fill: var(--button-hover-fill-color);\n",
              "  }\n",
              "\n",
              "  .colab-df-quickchart-complete:disabled,\n",
              "  .colab-df-quickchart-complete:disabled:hover {\n",
              "    background-color: var(--disabled-bg-color);\n",
              "    fill: var(--disabled-fill-color);\n",
              "    box-shadow: none;\n",
              "  }\n",
              "\n",
              "  .colab-df-spinner {\n",
              "    border: 2px solid var(--fill-color);\n",
              "    border-color: transparent;\n",
              "    border-bottom-color: var(--fill-color);\n",
              "    animation:\n",
              "      spin 1s steps(1) infinite;\n",
              "  }\n",
              "\n",
              "  @keyframes spin {\n",
              "    0% {\n",
              "      border-color: transparent;\n",
              "      border-bottom-color: var(--fill-color);\n",
              "      border-left-color: var(--fill-color);\n",
              "    }\n",
              "    20% {\n",
              "      border-color: transparent;\n",
              "      border-left-color: var(--fill-color);\n",
              "      border-top-color: var(--fill-color);\n",
              "    }\n",
              "    30% {\n",
              "      border-color: transparent;\n",
              "      border-left-color: var(--fill-color);\n",
              "      border-top-color: var(--fill-color);\n",
              "      border-right-color: var(--fill-color);\n",
              "    }\n",
              "    40% {\n",
              "      border-color: transparent;\n",
              "      border-right-color: var(--fill-color);\n",
              "      border-top-color: var(--fill-color);\n",
              "    }\n",
              "    60% {\n",
              "      border-color: transparent;\n",
              "      border-right-color: var(--fill-color);\n",
              "    }\n",
              "    80% {\n",
              "      border-color: transparent;\n",
              "      border-right-color: var(--fill-color);\n",
              "      border-bottom-color: var(--fill-color);\n",
              "    }\n",
              "    90% {\n",
              "      border-color: transparent;\n",
              "      border-bottom-color: var(--fill-color);\n",
              "    }\n",
              "  }\n",
              "</style>\n",
              "\n",
              "  <script>\n",
              "    async function quickchart(key) {\n",
              "      const quickchartButtonEl =\n",
              "        document.querySelector('#' + key + ' button');\n",
              "      quickchartButtonEl.disabled = true;  // To prevent multiple clicks.\n",
              "      quickchartButtonEl.classList.add('colab-df-spinner');\n",
              "      try {\n",
              "        const charts = await google.colab.kernel.invokeFunction(\n",
              "            'suggestCharts', [key], {});\n",
              "      } catch (error) {\n",
              "        console.error('Error during call to suggestCharts:', error);\n",
              "      }\n",
              "      quickchartButtonEl.classList.remove('colab-df-spinner');\n",
              "      quickchartButtonEl.classList.add('colab-df-quickchart-complete');\n",
              "    }\n",
              "    (() => {\n",
              "      let quickchartButtonEl =\n",
              "        document.querySelector('#df-2c099faf-dfe2-48a8-8cc6-bddf16fa8e6e button');\n",
              "      quickchartButtonEl.style.display =\n",
              "        google.colab.kernel.accessAllowed ? 'block' : 'none';\n",
              "    })();\n",
              "  </script>\n",
              "</div>\n",
              "\n",
              "    </div>\n",
              "  </div>\n"
            ]
          },
          "metadata": {},
          "execution_count": 36
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "IbK82N2gOdar",
        "outputId": "a4a3b85a-89da-482a-f1cc-66a14cc4cd3b"
      },
      "source": [
        "# checking the number of rows and columns in the dataframe\n",
        "mail_data.shape"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "(5572, 2)"
            ]
          },
          "metadata": {},
          "execution_count": 37
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "vhR4U3ATPBdk"
      },
      "source": [
        "Label Encoding"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "9EW7QSgeOt4p"
      },
      "source": [
        "# label spam mail as 0;  ham mail as 1;\n",
        "\n",
        "mail_data.loc[mail_data['Category'] == 'spam', 'Category',] = 0\n",
        "mail_data.loc[mail_data['Category'] == 'ham', 'Category',] = 1"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "uxZK1fWwPwII"
      },
      "source": [
        "spam  -  0\n",
        "\n",
        "ham  -  1"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "t8Rt-FaNPtPE"
      },
      "source": [
        "# separating the data as texts and label\n",
        "\n",
        "X = mail_data['Message']\n",
        "\n",
        "Y = mail_data['Category']"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "QnQeUBGtQPP7",
        "outputId": "b7401ba9-8af3-4fa2-eed0-467de9419d6b"
      },
      "source": [
        "print(X)"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "0       Go until jurong point, crazy.. Available only ...\n",
            "1                           Ok lar... Joking wif u oni...\n",
            "2       Free entry in 2 a wkly comp to win FA Cup fina...\n",
            "3       U dun say so early hor... U c already then say...\n",
            "4       Nah I don't think he goes to usf, he lives aro...\n",
            "                              ...                        \n",
            "5567    This is the 2nd time we have tried 2 contact u...\n",
            "5568                 Will ü b going to esplanade fr home?\n",
            "5569    Pity, * was in mood for that. So...any other s...\n",
            "5570    The guy did some bitching but I acted like i'd...\n",
            "5571                           Rofl. Its true to its name\n",
            "Name: Message, Length: 5572, dtype: object\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "cuWDNy5KQQjY",
        "outputId": "e791b936-a1c2-4c5f-81b9-ab8575d92645"
      },
      "source": [
        "print(Y)"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "0       1\n",
            "1       1\n",
            "2       0\n",
            "3       1\n",
            "4       1\n",
            "       ..\n",
            "5567    0\n",
            "5568    1\n",
            "5569    1\n",
            "5570    1\n",
            "5571    1\n",
            "Name: Category, Length: 5572, dtype: object\n"
          ]
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "jvHyqdH8QZPH"
      },
      "source": [
        "Splitting the data into training data & test data"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "RO2GmbSNQSQH"
      },
      "source": [
        "X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=0.2, random_state=3)"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "tS2c7A4NRa46",
        "outputId": "1af39b72-7b1f-4d16-c19b-885256113113"
      },
      "source": [
        "print(X.shape)\n",
        "print(X_train.shape)\n",
        "print(X_test.shape)"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "(5572,)\n",
            "(4457,)\n",
            "(1115,)\n"
          ]
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "wYQpiACGSBYM"
      },
      "source": [
        "Feature Extraction"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "nLs847nSRibm"
      },
      "source": [
        "# transform the text data to feature vectors that can be used as input to the Logistic regression\n",
        "\n",
        "feature_extraction = TfidfVectorizer(min_df = 1, stop_words='english', lowercase=True)\n",
        "\n",
        "X_train_features = feature_extraction.fit_transform(X_train)\n",
        "X_test_features = feature_extraction.transform(X_test)\n",
        "\n",
        "# convert Y_train and Y_test values as integers\n",
        "\n",
        "Y_train = Y_train.astype('int')\n",
        "Y_test = Y_test.astype('int')"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "print(X_train)"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "PmPOa7TqJEjT",
        "outputId": "206afdf8-c1c2-4d5f-8587-49b23b03bb80"
      },
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "3075                  Don know. I did't msg him recently.\n",
            "1787    Do you know why god created gap between your f...\n",
            "1614                         Thnx dude. u guys out 2nite?\n",
            "4304                                      Yup i'm free...\n",
            "3266    44 7732584351, Do you want a New Nokia 3510i c...\n",
            "                              ...                        \n",
            "789     5 Free Top Polyphonic Tones call 087018728737,...\n",
            "968     What do u want when i come back?.a beautiful n...\n",
            "1667    Guess who spent all last night phasing in and ...\n",
            "3321    Eh sorry leh... I din c ur msg. Not sad alread...\n",
            "1688    Free Top ringtone -sub to weekly ringtone-get ...\n",
            "Name: Message, Length: 4457, dtype: object\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "print(X_train_features)"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "hYwtl8Y5JQnI",
        "outputId": "2eb6a02e-35c3-4445-a67e-ac04f0c03457"
      },
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "  (0, 5413)\t0.6198254967574347\n",
            "  (0, 4456)\t0.4168658090846482\n",
            "  (0, 2224)\t0.413103377943378\n",
            "  (0, 3811)\t0.34780165336891333\n",
            "  (0, 2329)\t0.38783870336935383\n",
            "  (1, 4080)\t0.18880584110891163\n",
            "  (1, 3185)\t0.29694482957694585\n",
            "  (1, 3325)\t0.31610586766078863\n",
            "  (1, 2957)\t0.3398297002864083\n",
            "  (1, 2746)\t0.3398297002864083\n",
            "  (1, 918)\t0.22871581159877646\n",
            "  (1, 1839)\t0.2784903590561455\n",
            "  (1, 2758)\t0.3226407885943799\n",
            "  (1, 2956)\t0.33036995955537024\n",
            "  (1, 1991)\t0.33036995955537024\n",
            "  (1, 3046)\t0.2503712792613518\n",
            "  (1, 3811)\t0.17419952275504033\n",
            "  (2, 407)\t0.509272536051008\n",
            "  (2, 3156)\t0.4107239318312698\n",
            "  (2, 2404)\t0.45287711070606745\n",
            "  (2, 6601)\t0.6056811524587518\n",
            "  (3, 2870)\t0.5864269879324768\n",
            "  (3, 7414)\t0.8100020912469564\n",
            "  (4, 50)\t0.23633754072626942\n",
            "  (4, 5497)\t0.15743785051118356\n",
            "  :\t:\n",
            "  (4454, 4602)\t0.2669765732445391\n",
            "  (4454, 3142)\t0.32014451677763156\n",
            "  (4455, 2247)\t0.37052851863170466\n",
            "  (4455, 2469)\t0.35441545511837946\n",
            "  (4455, 5646)\t0.33545678464631296\n",
            "  (4455, 6810)\t0.29731757715898277\n",
            "  (4455, 6091)\t0.23103841516927642\n",
            "  (4455, 7113)\t0.30536590342067704\n",
            "  (4455, 3872)\t0.3108911491788658\n",
            "  (4455, 4715)\t0.30714144758811196\n",
            "  (4455, 6916)\t0.19636985317119715\n",
            "  (4455, 3922)\t0.31287563163368587\n",
            "  (4455, 4456)\t0.24920025316220423\n",
            "  (4456, 141)\t0.292943737785358\n",
            "  (4456, 647)\t0.30133182431707617\n",
            "  (4456, 6311)\t0.30133182431707617\n",
            "  (4456, 5569)\t0.4619395404299172\n",
            "  (4456, 6028)\t0.21034888000987115\n",
            "  (4456, 7154)\t0.24083218452280053\n",
            "  (4456, 7150)\t0.3677554681447669\n",
            "  (4456, 6249)\t0.17573831794959716\n",
            "  (4456, 6307)\t0.2752760476857975\n",
            "  (4456, 334)\t0.2220077711654938\n",
            "  (4456, 5778)\t0.16243064490100795\n",
            "  (4456, 2870)\t0.31523196273113385\n"
          ]
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "Training the Model"
      ],
      "metadata": {
        "id": "3z2ydanBJcdB"
      }
    },
    {
      "cell_type": "markdown",
      "source": [
        "Logistic Regression\n"
      ],
      "metadata": {
        "id": "Lm-6OrRgJfdc"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "model = LogisticRegression()"
      ],
      "metadata": {
        "id": "qVB_TfadJjiP"
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "#training the logistic Regression model with the training data\n",
        "model.fit(X_train_features,Y_train)"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 74
        },
        "id": "QktX8bh4Ju3N",
        "outputId": "67f43fd3-fbf3-40b7-963f-8f0951211f21"
      },
      "execution_count": null,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "LogisticRegression()"
            ],
            "text/html": [
              "<style>#sk-container-id-2 {color: black;background-color: white;}#sk-container-id-2 pre{padding: 0;}#sk-container-id-2 div.sk-toggleable {background-color: white;}#sk-container-id-2 label.sk-toggleable__label {cursor: pointer;display: block;width: 100%;margin-bottom: 0;padding: 0.3em;box-sizing: border-box;text-align: center;}#sk-container-id-2 label.sk-toggleable__label-arrow:before {content: \"▸\";float: left;margin-right: 0.25em;color: #696969;}#sk-container-id-2 label.sk-toggleable__label-arrow:hover:before {color: black;}#sk-container-id-2 div.sk-estimator:hover label.sk-toggleable__label-arrow:before {color: black;}#sk-container-id-2 div.sk-toggleable__content {max-height: 0;max-width: 0;overflow: hidden;text-align: left;background-color: #f0f8ff;}#sk-container-id-2 div.sk-toggleable__content pre {margin: 0.2em;color: black;border-radius: 0.25em;background-color: #f0f8ff;}#sk-container-id-2 input.sk-toggleable__control:checked~div.sk-toggleable__content {max-height: 200px;max-width: 100%;overflow: auto;}#sk-container-id-2 input.sk-toggleable__control:checked~label.sk-toggleable__label-arrow:before {content: \"▾\";}#sk-container-id-2 div.sk-estimator input.sk-toggleable__control:checked~label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-2 div.sk-label input.sk-toggleable__control:checked~label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-2 input.sk-hidden--visually {border: 0;clip: rect(1px 1px 1px 1px);clip: rect(1px, 1px, 1px, 1px);height: 1px;margin: -1px;overflow: hidden;padding: 0;position: absolute;width: 1px;}#sk-container-id-2 div.sk-estimator {font-family: monospace;background-color: #f0f8ff;border: 1px dotted black;border-radius: 0.25em;box-sizing: border-box;margin-bottom: 0.5em;}#sk-container-id-2 div.sk-estimator:hover {background-color: #d4ebff;}#sk-container-id-2 div.sk-parallel-item::after {content: \"\";width: 100%;border-bottom: 1px solid gray;flex-grow: 1;}#sk-container-id-2 div.sk-label:hover label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-2 div.sk-serial::before {content: \"\";position: absolute;border-left: 1px solid gray;box-sizing: border-box;top: 0;bottom: 0;left: 50%;z-index: 0;}#sk-container-id-2 div.sk-serial {display: flex;flex-direction: column;align-items: center;background-color: white;padding-right: 0.2em;padding-left: 0.2em;position: relative;}#sk-container-id-2 div.sk-item {position: relative;z-index: 1;}#sk-container-id-2 div.sk-parallel {display: flex;align-items: stretch;justify-content: center;background-color: white;position: relative;}#sk-container-id-2 div.sk-item::before, #sk-container-id-2 div.sk-parallel-item::before {content: \"\";position: absolute;border-left: 1px solid gray;box-sizing: border-box;top: 0;bottom: 0;left: 50%;z-index: -1;}#sk-container-id-2 div.sk-parallel-item {display: flex;flex-direction: column;z-index: 1;position: relative;background-color: white;}#sk-container-id-2 div.sk-parallel-item:first-child::after {align-self: flex-end;width: 50%;}#sk-container-id-2 div.sk-parallel-item:last-child::after {align-self: flex-start;width: 50%;}#sk-container-id-2 div.sk-parallel-item:only-child::after {width: 0;}#sk-container-id-2 div.sk-dashed-wrapped {border: 1px dashed gray;margin: 0 0.4em 0.5em 0.4em;box-sizing: border-box;padding-bottom: 0.4em;background-color: white;}#sk-container-id-2 div.sk-label label {font-family: monospace;font-weight: bold;display: inline-block;line-height: 1.2em;}#sk-container-id-2 div.sk-label-container {text-align: center;}#sk-container-id-2 div.sk-container {/* jupyter's `normalize.less` sets `[hidden] { display: none; }` but bootstrap.min.css set `[hidden] { display: none !important; }` so we also need the `!important` here to be able to override the default hidden behavior on the sphinx rendered scikit-learn.org. See: https://github.com/scikit-learn/scikit-learn/issues/21755 */display: inline-block !important;position: relative;}#sk-container-id-2 div.sk-text-repr-fallback {display: none;}</style><div id=\"sk-container-id-2\" class=\"sk-top-container\"><div class=\"sk-text-repr-fallback\"><pre>LogisticRegression()</pre><b>In a Jupyter environment, please rerun this cell to show the HTML representation or trust the notebook. <br />On GitHub, the HTML representation is unable to render, please try loading this page with nbviewer.org.</b></div><div class=\"sk-container\" hidden><div class=\"sk-item\"><div class=\"sk-estimator sk-toggleable\"><input class=\"sk-toggleable__control sk-hidden--visually\" id=\"sk-estimator-id-2\" type=\"checkbox\" checked><label for=\"sk-estimator-id-2\" class=\"sk-toggleable__label sk-toggleable__label-arrow\">LogisticRegression</label><div class=\"sk-toggleable__content\"><pre>LogisticRegression()</pre></div></div></div></div></div>"
            ]
          },
          "metadata": {},
          "execution_count": 51
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "Evaluting the trained model"
      ],
      "metadata": {
        "id": "-W-BVH6yKUYw"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "#prediction on training data\n",
        "\n",
        "\n",
        "prediction_on_training_data= model.predict(X_train_features)\n",
        "accuracy_on_training_data = accuracy_score(Y_train,prediction_on_training_data)"
      ],
      "metadata": {
        "id": "mbN8B_vxKRJd"
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "print('Accuracy on training data :',accuracy_on_training_data )"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "aV4tHD_dK97P",
        "outputId": "c6651dbb-28ec-4fef-fbc0-50e199897694"
      },
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "Accuracy on training data : 0.9670181736594121\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "source": [
        "#prediction on test data\n",
        "\n",
        "\n",
        "prediction_on_test_data= model.predict(X_train_features)\n",
        "accuracy_on_test_data = accuracy_score(Y_train,prediction_on_training_data)"
      ],
      "metadata": {
        "id": "rItmFZSRLWBv"
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "print('Accuracy on training data :',accuracy_on_test_data )"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "_XboA-dbL51n",
        "outputId": "fafe2ce5-fd27-4d2b-b084-c05a818f51b0"
      },
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "Accuracy on training data : 0.9670181736594121\n"
          ]
        }
      ]
    },
    {
      "cell_type": "markdown",
      "source": [
        "Building a Predictive system"
      ],
      "metadata": {
        "id": "euzzZAGqMofU"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "input_mail=[\"I've been searching for the right words to thank you for this breather. I promise i wont take your help for granted and will fulfil my promise. You have been wonderful and a blessing at all times.\"]\n",
        "\n",
        "#convert text to feature vectors\n",
        "input_data_features = feature_extraction.transform(input_mail)\n",
        "\n",
        "#making predection\n",
        "prediction = model.predict(input_data_features)\n",
        "print(prediction)\n",
        "if (prediction[0]==1):\n",
        "  print('Ham mail')\n",
        "else:\n",
        "    print('spam mail')"
      ],
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "l4D45NI5MsTt",
        "outputId": "5ee3435b-3408-415d-aba2-314e82ba64f0"
      },
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "[1]\n",
            "Ham mail\n"
          ]
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "AwbYw_asQxnY"
      },
      "source": [
        "Importing the Dependencies"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "bRMuN-m9Qxnf"
      },
      "source": [
        "import numpy as np\n",
        "import pandas as pd\n",
        "from sklearn.model_selection import train_test_split\n",
        "from sklearn.feature_extraction.text import TfidfVectorizer\n",
        "from sklearn.linear_model import LogisticRegression\n",
        "from sklearn.metrics import accuracy_score"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "E4PO-37aQxng"
      },
      "source": [
        "Data Collection & Pre-Processing"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "8_6u6NalQxng"
      },
      "source": [
        "# loading the data from csv file to a pandas Dataframe\n",
        "raw_mail_data = pd.read_csv('/content/mail_data.csv')"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "outputId": "28c19d96-23a2-43c0-86ad-5c1aee7f1b58",
        "id": "R5a9Ib1ZQxng"
      },
      "source": [
        "print(raw_mail_data)"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "     Category                                            Message\n",
            "0         ham  Go until jurong point, crazy.. Available only ...\n",
            "1         ham                      Ok lar... Joking wif u oni...\n",
            "2        spam  Free entry in 2 a wkly comp to win FA Cup fina...\n",
            "3         ham  U dun say so early hor... U c already then say...\n",
            "4         ham  Nah I don't think he goes to usf, he lives aro...\n",
            "...       ...                                                ...\n",
            "5567     spam  This is the 2nd time we have tried 2 contact u...\n",
            "5568      ham               Will ü b going to esplanade fr home?\n",
            "5569      ham  Pity, * was in mood for that. So...any other s...\n",
            "5570      ham  The guy did some bitching but I acted like i'd...\n",
            "5571      ham                         Rofl. Its true to its name\n",
            "\n",
            "[5572 rows x 2 columns]\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "kx0I02MpQxng"
      },
      "source": [
        "# replace the null values with a null string\n",
        "mail_data = raw_mail_data.where((pd.notnull(raw_mail_data)),'')"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 202
        },
        "outputId": "af1b0dfd-2ff9-4af9-cfcd-d0c177dd6ab9",
        "id": "V0JrVRWsQxnh"
      },
      "source": [
        "# printing the first 5 rows of the dataframe\n",
        "mail_data.head()"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/html": [
              "<div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>Category</th>\n",
              "      <th>Message</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>ham</td>\n",
              "      <td>Go until jurong point, crazy.. Available only ...</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>ham</td>\n",
              "      <td>Ok lar... Joking wif u oni...</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>spam</td>\n",
              "      <td>Free entry in 2 a wkly comp to win FA Cup fina...</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>ham</td>\n",
              "      <td>U dun say so early hor... U c already then say...</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>ham</td>\n",
              "      <td>Nah I don't think he goes to usf, he lives aro...</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>"
            ],
            "text/plain": [
              "  Category                                            Message\n",
              "0      ham  Go until jurong point, crazy.. Available only ...\n",
              "1      ham                      Ok lar... Joking wif u oni...\n",
              "2     spam  Free entry in 2 a wkly comp to win FA Cup fina...\n",
              "3      ham  U dun say so early hor... U c already then say...\n",
              "4      ham  Nah I don't think he goes to usf, he lives aro..."
            ]
          },
          "metadata": {},
          "execution_count": 5
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "outputId": "4d1840a1-22b5-468f-d4d0-a4528ef4313c",
        "id": "AzU1zoVBQxnh"
      },
      "source": [
        "# checking the number of rows and columns in the dataframe\n",
        "mail_data.shape"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "(5572, 2)"
            ]
          },
          "metadata": {},
          "execution_count": 6
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "WKNhk4teQxnh"
      },
      "source": [
        "Label Encoding"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "SJgEDzoJQxnh"
      },
      "source": [
        "# label spam mail as 0;  ham mail as 1;\n",
        "\n",
        "mail_data.loc[mail_data['Category'] == 'spam', 'Category',] = 0\n",
        "mail_data.loc[mail_data['Category'] == 'ham', 'Category',] = 1"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "5s05YzBgQxnh"
      },
      "source": [
        "spam  -  0\n",
        "\n",
        "ham  -  1"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "t_SBkzzeQxnh"
      },
      "source": [
        "# separating the data as texts and label\n",
        "\n",
        "X = mail_data['Message']\n",
        "\n",
        "Y = mail_data['Category']"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "outputId": "a2640f4b-2a1d-4742-9742-3ecbb6017668",
        "id": "RfAGA2XhQxnh"
      },
      "source": [
        "print(X)"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "0       Go until jurong point, crazy.. Available only ...\n",
            "1                           Ok lar... Joking wif u oni...\n",
            "2       Free entry in 2 a wkly comp to win FA Cup fina...\n",
            "3       U dun say so early hor... U c already then say...\n",
            "4       Nah I don't think he goes to usf, he lives aro...\n",
            "                              ...                        \n",
            "5567    This is the 2nd time we have tried 2 contact u...\n",
            "5568                 Will ü b going to esplanade fr home?\n",
            "5569    Pity, * was in mood for that. So...any other s...\n",
            "5570    The guy did some bitching but I acted like i'd...\n",
            "5571                           Rofl. Its true to its name\n",
            "Name: Message, Length: 5572, dtype: object\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "outputId": "1a0a109b-d63a-4cf0-fe4e-b486f1d3d623",
        "id": "4InIfro5Qxnh"
      },
      "source": [
        "print(Y)"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "0       1\n",
            "1       1\n",
            "2       0\n",
            "3       1\n",
            "4       1\n",
            "       ..\n",
            "5567    0\n",
            "5568    1\n",
            "5569    1\n",
            "5570    1\n",
            "5571    1\n",
            "Name: Category, Length: 5572, dtype: object\n"
          ]
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "IC_7WJOtQxni"
      },
      "source": [
        "Splitting the data into training data & test data"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "UoZMl1A8Qxni"
      },
      "source": [
        "X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=0.2, random_state=3)"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "outputId": "5d44247f-65d0-457d-8a94-0fd8b45a3b72",
        "id": "b49f8VBhQxni"
      },
      "source": [
        "print(X.shape)\n",
        "print(X_train.shape)\n",
        "print(X_test.shape)"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "(5572,)\n",
            "(4457,)\n",
            "(1115,)\n"
          ]
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "GIVh_1JFQxni"
      },
      "source": [
        "Feature Extraction"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "jiL6yD0mQxni"
      },
      "source": [
        "# transform the text data to feature vectors that can be used as input to the Logistic regression\n",
        "\n",
        "feature_extraction = TfidfVectorizer(min_df = 1, stop_words='english', lowercase='True')\n",
        "\n",
        "X_train_features = feature_extraction.fit_transform(X_train)\n",
        "X_test_features = feature_extraction.transform(X_test)\n",
        "\n",
        "# convert Y_train and Y_test values as integers\n",
        "\n",
        "Y_train = Y_train.astype('int')\n",
        "Y_test = Y_test.astype('int')"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "jmNca-orQzdt"
      },
      "source": [
        "Importing the Dependencies"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "WD3oXpAlQzeA"
      },
      "source": [
        "import numpy as np\n",
        "import pandas as pd\n",
        "from sklearn.model_selection import train_test_split\n",
        "from sklearn.feature_extraction.text import TfidfVectorizer\n",
        "from sklearn.linear_model import LogisticRegression\n",
        "from sklearn.metrics import accuracy_score"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "VDlvFjTQQzeA"
      },
      "source": [
        "Data Collection & Pre-Processing"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "sIuHNYIQQzeA"
      },
      "source": [
        "# loading the data from csv file to a pandas Dataframe\n",
        "raw_mail_data = pd.read_csv('/content/mail_data.csv')"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "outputId": "28c19d96-23a2-43c0-86ad-5c1aee7f1b58",
        "id": "2B8UZ5enQzeA"
      },
      "source": [
        "print(raw_mail_data)"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "     Category                                            Message\n",
            "0         ham  Go until jurong point, crazy.. Available only ...\n",
            "1         ham                      Ok lar... Joking wif u oni...\n",
            "2        spam  Free entry in 2 a wkly comp to win FA Cup fina...\n",
            "3         ham  U dun say so early hor... U c already then say...\n",
            "4         ham  Nah I don't think he goes to usf, he lives aro...\n",
            "...       ...                                                ...\n",
            "5567     spam  This is the 2nd time we have tried 2 contact u...\n",
            "5568      ham               Will ü b going to esplanade fr home?\n",
            "5569      ham  Pity, * was in mood for that. So...any other s...\n",
            "5570      ham  The guy did some bitching but I acted like i'd...\n",
            "5571      ham                         Rofl. Its true to its name\n",
            "\n",
            "[5572 rows x 2 columns]\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "BdU_rlL_QzeA"
      },
      "source": [
        "# replace the null values with a null string\n",
        "mail_data = raw_mail_data.where((pd.notnull(raw_mail_data)),'')"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 202
        },
        "outputId": "af1b0dfd-2ff9-4af9-cfcd-d0c177dd6ab9",
        "id": "veScjgHZQzeA"
      },
      "source": [
        "# printing the first 5 rows of the dataframe\n",
        "mail_data.head()"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/html": [
              "<div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>Category</th>\n",
              "      <th>Message</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>ham</td>\n",
              "      <td>Go until jurong point, crazy.. Available only ...</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>ham</td>\n",
              "      <td>Ok lar... Joking wif u oni...</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>spam</td>\n",
              "      <td>Free entry in 2 a wkly comp to win FA Cup fina...</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>ham</td>\n",
              "      <td>U dun say so early hor... U c already then say...</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>ham</td>\n",
              "      <td>Nah I don't think he goes to usf, he lives aro...</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>"
            ],
            "text/plain": [
              "  Category                                            Message\n",
              "0      ham  Go until jurong point, crazy.. Available only ...\n",
              "1      ham                      Ok lar... Joking wif u oni...\n",
              "2     spam  Free entry in 2 a wkly comp to win FA Cup fina...\n",
              "3      ham  U dun say so early hor... U c already then say...\n",
              "4      ham  Nah I don't think he goes to usf, he lives aro..."
            ]
          },
          "metadata": {},
          "execution_count": 5
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "outputId": "4d1840a1-22b5-468f-d4d0-a4528ef4313c",
        "id": "AJGqZfGBQzeA"
      },
      "source": [
        "# checking the number of rows and columns in the dataframe\n",
        "mail_data.shape"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "(5572, 2)"
            ]
          },
          "metadata": {},
          "execution_count": 6
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "GNa3p77FQzeA"
      },
      "source": [
        "Label Encoding"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "HKEcpMl0QzeB"
      },
      "source": [
        "# label spam mail as 0;  ham mail as 1;\n",
        "\n",
        "mail_data.loc[mail_data['Category'] == 'spam', 'Category',] = 0\n",
        "mail_data.loc[mail_data['Category'] == 'ham', 'Category',] = 1"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "5cywttCVQzeB"
      },
      "source": [
        "spam  -  0\n",
        "\n",
        "ham  -  1"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "3s7WNL9EQzeB"
      },
      "source": [
        "# separating the data as texts and label\n",
        "\n",
        "X = mail_data['Message']\n",
        "\n",
        "Y = mail_data['Category']"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "outputId": "a2640f4b-2a1d-4742-9742-3ecbb6017668",
        "id": "W1H00GaKQzeB"
      },
      "source": [
        "print(X)"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "0       Go until jurong point, crazy.. Available only ...\n",
            "1                           Ok lar... Joking wif u oni...\n",
            "2       Free entry in 2 a wkly comp to win FA Cup fina...\n",
            "3       U dun say so early hor... U c already then say...\n",
            "4       Nah I don't think he goes to usf, he lives aro...\n",
            "                              ...                        \n",
            "5567    This is the 2nd time we have tried 2 contact u...\n",
            "5568                 Will ü b going to esplanade fr home?\n",
            "5569    Pity, * was in mood for that. So...any other s...\n",
            "5570    The guy did some bitching but I acted like i'd...\n",
            "5571                           Rofl. Its true to its name\n",
            "Name: Message, Length: 5572, dtype: object\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "outputId": "1a0a109b-d63a-4cf0-fe4e-b486f1d3d623",
        "id": "VEPFpkNyQzeB"
      },
      "source": [
        "print(Y)"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "0       1\n",
            "1       1\n",
            "2       0\n",
            "3       1\n",
            "4       1\n",
            "       ..\n",
            "5567    0\n",
            "5568    1\n",
            "5569    1\n",
            "5570    1\n",
            "5571    1\n",
            "Name: Category, Length: 5572, dtype: object\n"
          ]
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "Pvmwmao8QzeB"
      },
      "source": [
        "Splitting the data into training data & test data"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "08aji9s8QzeB"
      },
      "source": [
        "X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=0.2, random_state=3)"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "outputId": "5d44247f-65d0-457d-8a94-0fd8b45a3b72",
        "id": "qKVcg5E9QzeB"
      },
      "source": [
        "print(X.shape)\n",
        "print(X_train.shape)\n",
        "print(X_test.shape)"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "(5572,)\n",
            "(4457,)\n",
            "(1115,)\n"
          ]
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "lX-PX3g4QzeB"
      },
      "source": [
        "Feature Extraction"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "M1bbBYVvQzeB"
      },
      "source": [
        "# transform the text data to feature vectors that can be used as input to the Logistic regression\n",
        "\n",
        "feature_extraction = TfidfVectorizer(min_df = 1, stop_words='english', lowercase=True)\n",
        "\n",
        "X_train_features = feature_extraction.fit_transform(X_train)\n",
        "X_test_features = feature_extraction.transform(X_test)\n",
        "\n",
        "# convert Y_train and Y_test values as integers\n",
        "\n",
        "Y_train = Y_train.astype('int')\n",
        "Y_test = Y_test.astype('int')"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "dBMAcw9RUkUY",
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "outputId": "a5ff6b83-f077-4227-af66-4a7f35e1896a"
      },
      "source": [
        "print(X_train)"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "3075                  Don know. I did't msg him recently.\n",
            "1787    Do you know why god created gap between your f...\n",
            "1614                         Thnx dude. u guys out 2nite?\n",
            "4304                                      Yup i'm free...\n",
            "3266    44 7732584351, Do you want a New Nokia 3510i c...\n",
            "                              ...                        \n",
            "789     5 Free Top Polyphonic Tones call 087018728737,...\n",
            "968     What do u want when i come back?.a beautiful n...\n",
            "1667    Guess who spent all last night phasing in and ...\n",
            "3321    Eh sorry leh... I din c ur msg. Not sad alread...\n",
            "1688    Free Top ringtone -sub to weekly ringtone-get ...\n",
            "Name: Message, Length: 4457, dtype: object\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "1NFuGogZUpt0",
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "outputId": "3c988708-32c3-402b-f100-0e46479d52f2"
      },
      "source": [
        "print(X_train_features)"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "  (0, 5413)\t0.6198254967574347\n",
            "  (0, 4456)\t0.4168658090846482\n",
            "  (0, 2224)\t0.413103377943378\n",
            "  (0, 3811)\t0.34780165336891333\n",
            "  (0, 2329)\t0.38783870336935383\n",
            "  (1, 4080)\t0.18880584110891163\n",
            "  (1, 3185)\t0.29694482957694585\n",
            "  (1, 3325)\t0.31610586766078863\n",
            "  (1, 2957)\t0.3398297002864083\n",
            "  (1, 2746)\t0.3398297002864083\n",
            "  (1, 918)\t0.22871581159877646\n",
            "  (1, 1839)\t0.2784903590561455\n",
            "  (1, 2758)\t0.3226407885943799\n",
            "  (1, 2956)\t0.33036995955537024\n",
            "  (1, 1991)\t0.33036995955537024\n",
            "  (1, 3046)\t0.2503712792613518\n",
            "  (1, 3811)\t0.17419952275504033\n",
            "  (2, 407)\t0.509272536051008\n",
            "  (2, 3156)\t0.4107239318312698\n",
            "  (2, 2404)\t0.45287711070606745\n",
            "  (2, 6601)\t0.6056811524587518\n",
            "  (3, 2870)\t0.5864269879324768\n",
            "  (3, 7414)\t0.8100020912469564\n",
            "  (4, 50)\t0.23633754072626942\n",
            "  (4, 5497)\t0.15743785051118356\n",
            "  :\t:\n",
            "  (4454, 4602)\t0.2669765732445391\n",
            "  (4454, 3142)\t0.32014451677763156\n",
            "  (4455, 2247)\t0.37052851863170466\n",
            "  (4455, 2469)\t0.35441545511837946\n",
            "  (4455, 5646)\t0.33545678464631296\n",
            "  (4455, 6810)\t0.29731757715898277\n",
            "  (4455, 6091)\t0.23103841516927642\n",
            "  (4455, 7113)\t0.30536590342067704\n",
            "  (4455, 3872)\t0.3108911491788658\n",
            "  (4455, 4715)\t0.30714144758811196\n",
            "  (4455, 6916)\t0.19636985317119715\n",
            "  (4455, 3922)\t0.31287563163368587\n",
            "  (4455, 4456)\t0.24920025316220423\n",
            "  (4456, 141)\t0.292943737785358\n",
            "  (4456, 647)\t0.30133182431707617\n",
            "  (4456, 6311)\t0.30133182431707617\n",
            "  (4456, 5569)\t0.4619395404299172\n",
            "  (4456, 6028)\t0.21034888000987115\n",
            "  (4456, 7154)\t0.24083218452280053\n",
            "  (4456, 7150)\t0.3677554681447669\n",
            "  (4456, 6249)\t0.17573831794959716\n",
            "  (4456, 6307)\t0.2752760476857975\n",
            "  (4456, 334)\t0.2220077711654938\n",
            "  (4456, 5778)\t0.16243064490100795\n",
            "  (4456, 2870)\t0.31523196273113385\n"
          ]
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "q86FvELbU_SV"
      },
      "source": [
        "Training the Model"
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "hV6BAIZQVBbo"
      },
      "source": [
        "Logistic Regression"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "1JeAOwzpUv0V"
      },
      "source": [
        "model = LogisticRegression()"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 74
        },
        "id": "gWGRHWAPVI_z",
        "outputId": "64bd9eac-bb57-4c7d-9555-5e1fc896d786"
      },
      "source": [
        "# training the Logistic Regression model with the training data\n",
        "model.fit(X_train_features, Y_train)"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "LogisticRegression()"
            ],
            "text/html": [
              "<style>#sk-container-id-3 {color: black;background-color: white;}#sk-container-id-3 pre{padding: 0;}#sk-container-id-3 div.sk-toggleable {background-color: white;}#sk-container-id-3 label.sk-toggleable__label {cursor: pointer;display: block;width: 100%;margin-bottom: 0;padding: 0.3em;box-sizing: border-box;text-align: center;}#sk-container-id-3 label.sk-toggleable__label-arrow:before {content: \"▸\";float: left;margin-right: 0.25em;color: #696969;}#sk-container-id-3 label.sk-toggleable__label-arrow:hover:before {color: black;}#sk-container-id-3 div.sk-estimator:hover label.sk-toggleable__label-arrow:before {color: black;}#sk-container-id-3 div.sk-toggleable__content {max-height: 0;max-width: 0;overflow: hidden;text-align: left;background-color: #f0f8ff;}#sk-container-id-3 div.sk-toggleable__content pre {margin: 0.2em;color: black;border-radius: 0.25em;background-color: #f0f8ff;}#sk-container-id-3 input.sk-toggleable__control:checked~div.sk-toggleable__content {max-height: 200px;max-width: 100%;overflow: auto;}#sk-container-id-3 input.sk-toggleable__control:checked~label.sk-toggleable__label-arrow:before {content: \"▾\";}#sk-container-id-3 div.sk-estimator input.sk-toggleable__control:checked~label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-3 div.sk-label input.sk-toggleable__control:checked~label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-3 input.sk-hidden--visually {border: 0;clip: rect(1px 1px 1px 1px);clip: rect(1px, 1px, 1px, 1px);height: 1px;margin: -1px;overflow: hidden;padding: 0;position: absolute;width: 1px;}#sk-container-id-3 div.sk-estimator {font-family: monospace;background-color: #f0f8ff;border: 1px dotted black;border-radius: 0.25em;box-sizing: border-box;margin-bottom: 0.5em;}#sk-container-id-3 div.sk-estimator:hover {background-color: #d4ebff;}#sk-container-id-3 div.sk-parallel-item::after {content: \"\";width: 100%;border-bottom: 1px solid gray;flex-grow: 1;}#sk-container-id-3 div.sk-label:hover label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-3 div.sk-serial::before {content: \"\";position: absolute;border-left: 1px solid gray;box-sizing: border-box;top: 0;bottom: 0;left: 50%;z-index: 0;}#sk-container-id-3 div.sk-serial {display: flex;flex-direction: column;align-items: center;background-color: white;padding-right: 0.2em;padding-left: 0.2em;position: relative;}#sk-container-id-3 div.sk-item {position: relative;z-index: 1;}#sk-container-id-3 div.sk-parallel {display: flex;align-items: stretch;justify-content: center;background-color: white;position: relative;}#sk-container-id-3 div.sk-item::before, #sk-container-id-3 div.sk-parallel-item::before {content: \"\";position: absolute;border-left: 1px solid gray;box-sizing: border-box;top: 0;bottom: 0;left: 50%;z-index: -1;}#sk-container-id-3 div.sk-parallel-item {display: flex;flex-direction: column;z-index: 1;position: relative;background-color: white;}#sk-container-id-3 div.sk-parallel-item:first-child::after {align-self: flex-end;width: 50%;}#sk-container-id-3 div.sk-parallel-item:last-child::after {align-self: flex-start;width: 50%;}#sk-container-id-3 div.sk-parallel-item:only-child::after {width: 0;}#sk-container-id-3 div.sk-dashed-wrapped {border: 1px dashed gray;margin: 0 0.4em 0.5em 0.4em;box-sizing: border-box;padding-bottom: 0.4em;background-color: white;}#sk-container-id-3 div.sk-label label {font-family: monospace;font-weight: bold;display: inline-block;line-height: 1.2em;}#sk-container-id-3 div.sk-label-container {text-align: center;}#sk-container-id-3 div.sk-container {/* jupyter's `normalize.less` sets `[hidden] { display: none; }` but bootstrap.min.css set `[hidden] { display: none !important; }` so we also need the `!important` here to be able to override the default hidden behavior on the sphinx rendered scikit-learn.org. See: https://github.com/scikit-learn/scikit-learn/issues/21755 */display: inline-block !important;position: relative;}#sk-container-id-3 div.sk-text-repr-fallback {display: none;}</style><div id=\"sk-container-id-3\" class=\"sk-top-container\"><div class=\"sk-text-repr-fallback\"><pre>LogisticRegression()</pre><b>In a Jupyter environment, please rerun this cell to show the HTML representation or trust the notebook. <br />On GitHub, the HTML representation is unable to render, please try loading this page with nbviewer.org.</b></div><div class=\"sk-container\" hidden><div class=\"sk-item\"><div class=\"sk-estimator sk-toggleable\"><input class=\"sk-toggleable__control sk-hidden--visually\" id=\"sk-estimator-id-3\" type=\"checkbox\" checked><label for=\"sk-estimator-id-3\" class=\"sk-toggleable__label sk-toggleable__label-arrow\">LogisticRegression</label><div class=\"sk-toggleable__content\"><pre>LogisticRegression()</pre></div></div></div></div></div>"
            ]
          },
          "metadata": {},
          "execution_count": 73
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "wZ01fa8dVeL5"
      },
      "source": [
        "Evaluating the trained model"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "ExiF2kKxVYtC"
      },
      "source": [
        "# prediction on training data\n",
        "\n",
        "prediction_on_training_data = model.predict(X_train_features)\n",
        "accuracy_on_training_data = accuracy_score(Y_train, prediction_on_training_data)"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "o7t4DI5UWCkB",
        "outputId": "a0b06af4-30f0-4c9d-b3a8-8894a5212c24"
      },
      "source": [
        "print('Accuracy on training data : ', accuracy_on_training_data)"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "Accuracy on training data :  0.9670181736594121\n"
          ]
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "cTin5rXTWKg3"
      },
      "source": [
        "# prediction on test data\n",
        "\n",
        "prediction_on_test_data = model.predict(X_test_features)\n",
        "accuracy_on_test_data = accuracy_score(Y_test, prediction_on_test_data)"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "4gvoMK4OWnJY",
        "outputId": "a9fedca7-9d4d-416f-f67d-caa8f8fb07e3"
      },
      "source": [
        "print('Accuracy on test data : ', accuracy_on_test_data)"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "Accuracy on test data :  0.9659192825112107\n"
          ]
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "bXdOKxYAXaHC"
      },
      "source": [
        "Building a Predictive System"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "h60z1__mWql6",
        "outputId": "386e2842-c6b5-4e41-d89b-052245287cf1"
      },
      "source": [
        "input_mail = [\"I've been searching for the right words to thank you for this breather. I promise i wont take your help for granted and will fulfil my promise. You have been wonderful and a blessing at all times\"]\n",
        "\n",
        "# convert text to feature vectors\n",
        "input_data_features = feature_extraction.transform(input_mail)\n",
        "\n",
        "# making prediction\n",
        "\n",
        "prediction = model.predict(input_data_features)\n",
        "print(prediction)\n",
        "\n",
        "\n",
        "if (prediction[0]==1):\n",
        "  print('Ham mail')\n",
        "\n",
        "else:\n",
        "  print('Spam mail')"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "[1]\n",
            "Ham mail\n"
          ]
        }
      ]
    }
  ]
}











