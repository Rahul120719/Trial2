# Trial2
{
  "nbformat": 4,
  "nbformat_minor": 0,
  "metadata": {
    "colab": {
      "name": "CDD-ML-Part-1-bioactivity-data.ipynb",
      "provenance": [],
      "collapsed_sections": []
    },
    "kernelspec": {
      "name": "python3",
      "display_name": "Python 3"
    }
  },
  "cells": [
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "wSFbIMb87cHu",
        "colab_type": "text"
      },
      "source": [
        "# **Computational Drug Discovery [Part 1] Download Bioactivity Data**\n",
        "\n",
        "Chanin Nantasenamat\n",
        "\n",
        "[*'Data Professor' YouTube channel*](http://youtube.com/dataprofessor)\n",
        "\n",
        "In this Jupyter notebook, we will be building a real-life **data science project** that you can include in your **data science portfolio**. Particularly, we will be building a machine learning model using the ChEMBL bioactivity data.\n",
        "\n",
        "---"
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "3iQiERxumDor",
        "colab_type": "text"
      },
      "source": [
        "## **ChEMBL Database**\n",
        "\n",
        "The [*ChEMBL Database*](https://www.ebi.ac.uk/chembl/) is a database that contains curated bioactivity data of more than 2 million compounds. It is compiled from more than 76,000 documents, 1.2 million assays and the data spans 13,000 targets and 1,800 cells and 33,000 indications.\n",
        "[Data as of March 25, 2020; ChEMBL version 26]."
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "iryGAwAIQ4yf",
        "colab_type": "text"
      },
      "source": [
        "## **Installing libraries**"
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "toGT1U_B7F2i",
        "colab_type": "text"
      },
      "source": [
        "Install the ChEMBL web service package so that we can retrieve bioactivity data from the ChEMBL Database."
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "cJGExHQBfLh7",
        "colab_type": "code",
        "outputId": "783c9cb5-c5d4-4545-a9d3-6c2a2f2b0e53",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 349
        }
      },
      "source": [
        "! pip install chembl_webresource_client"
      ],
      "execution_count": 0,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "Collecting chembl_webresource_client\n",
            "\u001b[?25l  Downloading https://files.pythonhosted.org/packages/74/c4/6526156c7e2f164a0fc061aae20d383f0b6b1e79957510a64382e676e2dc/chembl-webresource-client-0.10.1.tar.gz (53kB)\n",
            "\r\u001b[K     |██████▏                         | 10kB 18.6MB/s eta 0:00:01\r\u001b[K     |████████████▎                   | 20kB 6.7MB/s eta 0:00:01\r\u001b[K     |██████████████████▍             | 30kB 7.8MB/s eta 0:00:01\r\u001b[K     |████████████████████████▌       | 40kB 8.4MB/s eta 0:00:01\r\u001b[K     |██████████████████████████████▊ | 51kB 7.2MB/s eta 0:00:01\r\u001b[K     |████████████████████████████████| 61kB 4.6MB/s \n",
            "\u001b[?25hRequirement already satisfied: urllib3 in /usr/local/lib/python3.6/dist-packages (from chembl_webresource_client) (1.24.3)\n",
            "Requirement already satisfied: requests>=2.18.4 in /usr/local/lib/python3.6/dist-packages (from chembl_webresource_client) (2.21.0)\n",
            "Collecting requests-cache>=0.4.7\n",
            "  Downloading https://files.pythonhosted.org/packages/7f/55/9b1c40eb83c16d8fc79c5f6c2ffade04208b080670fbfc35e0a5effb5a92/requests_cache-0.5.2-py2.py3-none-any.whl\n",
            "Requirement already satisfied: easydict in /usr/local/lib/python3.6/dist-packages (from chembl_webresource_client) (1.9)\n",
            "Requirement already satisfied: chardet<3.1.0,>=3.0.2 in /usr/local/lib/python3.6/dist-packages (from requests>=2.18.4->chembl_webresource_client) (3.0.4)\n",
            "Requirement already satisfied: certifi>=2017.4.17 in /usr/local/lib/python3.6/dist-packages (from requests>=2.18.4->chembl_webresource_client) (2020.4.5.1)\n",
            "Requirement already satisfied: idna<2.9,>=2.5 in /usr/local/lib/python3.6/dist-packages (from requests>=2.18.4->chembl_webresource_client) (2.8)\n",
            "Building wheels for collected packages: chembl-webresource-client\n",
            "  Building wheel for chembl-webresource-client (setup.py) ... \u001b[?25l\u001b[?25hdone\n",
            "  Created wheel for chembl-webresource-client: filename=chembl_webresource_client-0.10.1-cp36-none-any.whl size=57153 sha256=c1e2f3e1514ff0b430f04c4dfe2d1bf8fc00e424b9883d7caae9ac0f6d94c36a\n",
            "  Stored in directory: /root/.cache/pip/wheels/81/8e/3b/4ec9940a01673307821600bfac28b17971caf84ff2b64653cb\n",
            "Successfully built chembl-webresource-client\n",
            "Installing collected packages: requests-cache, chembl-webresource-client\n",
            "Successfully installed chembl-webresource-client-0.10.1 requests-cache-0.5.2\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "J0kJjL8gb5nX",
        "colab_type": "text"
      },
      "source": [
        "## **Importing libraries**"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "RXoCvMPPfNrv",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "# Import necessary libraries\n",
        "import pandas as pd\n",
        "from chembl_webresource_client.new_client import new_client"
      ],
      "execution_count": 0,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "1FgUai1bfigC",
        "colab_type": "text"
      },
      "source": [
        "## **Search for Target protein**"
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "7lBsDrD0gAqH",
        "colab_type": "text"
      },
      "source": [
        "### **Target search for coronavirus**"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "Vxtp79so4ZjF",
        "colab_type": "code",
        "outputId": "e90dde45-1c0d-4fd9-f693-cb6e6032e2cd",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 145
        }
      },
      "source": [
        "# Target search for coronavirus\n",
        "target = new_client.target\n",
        "target_query = target.search('aromatase')\n",
        "targets = pd.DataFrame.from_dict(target_query)\n",
        "targets"
      ],
      "execution_count": 0,
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
              "      <th>cross_references</th>\n",
              "      <th>organism</th>\n",
              "      <th>pref_name</th>\n",
              "      <th>score</th>\n",
              "      <th>species_group_flag</th>\n",
              "      <th>target_chembl_id</th>\n",
              "      <th>target_components</th>\n",
              "      <th>target_type</th>\n",
              "      <th>tax_id</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>[{'xref_id': 'P11511', 'xref_name': None, 'xre...</td>\n",
              "      <td>Homo sapiens</td>\n",
              "      <td>Cytochrome P450 19A1</td>\n",
              "      <td>19.0</td>\n",
              "      <td>False</td>\n",
              "      <td>CHEMBL1978</td>\n",
              "      <td>[{'accession': 'P11511', 'component_descriptio...</td>\n",
              "      <td>SINGLE PROTEIN</td>\n",
              "      <td>9606</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>[{'xref_id': 'P22443', 'xref_name': None, 'xre...</td>\n",
              "      <td>Rattus norvegicus</td>\n",
              "      <td>Cytochrome P450 19A1</td>\n",
              "      <td>19.0</td>\n",
              "      <td>False</td>\n",
              "      <td>CHEMBL3859</td>\n",
              "      <td>[{'accession': 'P22443', 'component_descriptio...</td>\n",
              "      <td>SINGLE PROTEIN</td>\n",
              "      <td>10116</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>"
            ],
            "text/plain": [
              "                                    cross_references  ... tax_id\n",
              "0  [{'xref_id': 'P11511', 'xref_name': None, 'xre...  ...   9606\n",
              "1  [{'xref_id': 'P22443', 'xref_name': None, 'xre...  ...  10116\n",
              "\n",
              "[2 rows x 9 columns]"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 42
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "Y5OPfEALjAfZ",
        "colab_type": "text"
      },
      "source": [
        "### **Select and retrieve bioactivity data for *SARS coronavirus 3C-like proteinase* (fifth entry)**"
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "gSQ3aroOgML7",
        "colab_type": "text"
      },
      "source": [
        "We will assign the fifth entry (which corresponds to the target protein, *coronavirus 3C-like proteinase*) to the ***selected_target*** variable "
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "StrcHMVLha7u",
        "colab_type": "code",
        "outputId": "a558535b-c66a-42ce-8604-3cf34dbff90b",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 35
        }
      },
      "source": [
        "selected_target = targets.target_chembl_id[4]\n",
        "selected_target"
      ],
      "execution_count": 0,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "'CHEMBL3927'"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 4
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "GWd2DRalgjzB",
        "colab_type": "text"
      },
      "source": [
        "Here, we will retrieve only bioactivity data for *coronavirus 3C-like proteinase* (CHEMBL3927) that are reported as IC$_{50}$ values in nM (nanomolar) unit."
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "LeFbV_CsSP8D",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "activity = new_client.activity\n",
        "res = activity.filter(target_chembl_id=selected_target).filter(standard_type=\"IC50\")"
      ],
      "execution_count": 0,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "RC4T-NEmSWV-",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "df = pd.DataFrame.from_dict(res)"
      ],
      "execution_count": 0,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "s9iUAXFdSkoM",
        "colab_type": "code",
        "outputId": "c9b681cc-97ab-40fb-a735-5e2b39612f8c",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 265
        }
      },
      "source": [
        "df.head(3)"
      ],
      "execution_count": 0,
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
              "      <th>activity_comment</th>\n",
              "      <th>activity_id</th>\n",
              "      <th>activity_properties</th>\n",
              "      <th>assay_chembl_id</th>\n",
              "      <th>assay_description</th>\n",
              "      <th>assay_type</th>\n",
              "      <th>bao_endpoint</th>\n",
              "      <th>bao_format</th>\n",
              "      <th>bao_label</th>\n",
              "      <th>canonical_smiles</th>\n",
              "      <th>data_validity_comment</th>\n",
              "      <th>data_validity_description</th>\n",
              "      <th>document_chembl_id</th>\n",
              "      <th>document_journal</th>\n",
              "      <th>document_year</th>\n",
              "      <th>ligand_efficiency</th>\n",
              "      <th>molecule_chembl_id</th>\n",
              "      <th>molecule_pref_name</th>\n",
              "      <th>parent_molecule_chembl_id</th>\n",
              "      <th>pchembl_value</th>\n",
              "      <th>potential_duplicate</th>\n",
              "      <th>qudt_units</th>\n",
              "      <th>record_id</th>\n",
              "      <th>relation</th>\n",
              "      <th>src_id</th>\n",
              "      <th>standard_flag</th>\n",
              "      <th>standard_relation</th>\n",
              "      <th>standard_text_value</th>\n",
              "      <th>standard_type</th>\n",
              "      <th>standard_units</th>\n",
              "      <th>standard_upper_value</th>\n",
              "      <th>standard_value</th>\n",
              "      <th>target_chembl_id</th>\n",
              "      <th>target_organism</th>\n",
              "      <th>target_pref_name</th>\n",
              "      <th>target_tax_id</th>\n",
              "      <th>text_value</th>\n",
              "      <th>toid</th>\n",
              "      <th>type</th>\n",
              "      <th>units</th>\n",
              "      <th>uo_units</th>\n",
              "      <th>upper_value</th>\n",
              "      <th>value</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>None</td>\n",
              "      <td>1480935</td>\n",
              "      <td>[]</td>\n",
              "      <td>CHEMBL829584</td>\n",
              "      <td>In vitro inhibitory concentration against SARS...</td>\n",
              "      <td>B</td>\n",
              "      <td>BAO_0000190</td>\n",
              "      <td>BAO_0000357</td>\n",
              "      <td>single protein format</td>\n",
              "      <td>Cc1noc(C)c1CN1C(=O)C(=O)c2cc(C#N)ccc21</td>\n",
              "      <td>None</td>\n",
              "      <td>None</td>\n",
              "      <td>CHEMBL1139624</td>\n",
              "      <td>Bioorg. Med. Chem. Lett.</td>\n",
              "      <td>2005</td>\n",
              "      <td>{'bei': '18.28', 'le': '0.33', 'lle': '3.25', ...</td>\n",
              "      <td>CHEMBL187579</td>\n",
              "      <td>None</td>\n",
              "      <td>CHEMBL187579</td>\n",
              "      <td>5.14</td>\n",
              "      <td>False</td>\n",
              "      <td>http://www.openphacts.org/units/Nanomolar</td>\n",
              "      <td>384103</td>\n",
              "      <td>=</td>\n",
              "      <td>1</td>\n",
              "      <td>True</td>\n",
              "      <td>=</td>\n",
              "      <td>None</td>\n",
              "      <td>IC50</td>\n",
              "      <td>nM</td>\n",
              "      <td>None</td>\n",
              "      <td>7200.0</td>\n",
              "      <td>CHEMBL3927</td>\n",
              "      <td>SARS coronavirus</td>\n",
              "      <td>SARS coronavirus 3C-like proteinase</td>\n",
              "      <td>227859</td>\n",
              "      <td>None</td>\n",
              "      <td>None</td>\n",
              "      <td>IC50</td>\n",
              "      <td>uM</td>\n",
              "      <td>UO_0000065</td>\n",
              "      <td>None</td>\n",
              "      <td>7.2</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>None</td>\n",
              "      <td>1480936</td>\n",
              "      <td>[]</td>\n",
              "      <td>CHEMBL829584</td>\n",
              "      <td>In vitro inhibitory concentration against SARS...</td>\n",
              "      <td>B</td>\n",
              "      <td>BAO_0000190</td>\n",
              "      <td>BAO_0000357</td>\n",
              "      <td>single protein format</td>\n",
              "      <td>O=C1C(=O)N(Cc2ccc(F)cc2Cl)c2ccc(I)cc21</td>\n",
              "      <td>None</td>\n",
              "      <td>None</td>\n",
              "      <td>CHEMBL1139624</td>\n",
              "      <td>Bioorg. Med. Chem. Lett.</td>\n",
              "      <td>2005</td>\n",
              "      <td>{'bei': '12.10', 'le': '0.33', 'lle': '1.22', ...</td>\n",
              "      <td>CHEMBL188487</td>\n",
              "      <td>None</td>\n",
              "      <td>CHEMBL188487</td>\n",
              "      <td>5.03</td>\n",
              "      <td>False</td>\n",
              "      <td>http://www.openphacts.org/units/Nanomolar</td>\n",
              "      <td>383984</td>\n",
              "      <td>=</td>\n",
              "      <td>1</td>\n",
              "      <td>True</td>\n",
              "      <td>=</td>\n",
              "      <td>None</td>\n",
              "      <td>IC50</td>\n",
              "      <td>nM</td>\n",
              "      <td>None</td>\n",
              "      <td>9400.0</td>\n",
              "      <td>CHEMBL3927</td>\n",
              "      <td>SARS coronavirus</td>\n",
              "      <td>SARS coronavirus 3C-like proteinase</td>\n",
              "      <td>227859</td>\n",
              "      <td>None</td>\n",
              "      <td>None</td>\n",
              "      <td>IC50</td>\n",
              "      <td>uM</td>\n",
              "      <td>UO_0000065</td>\n",
              "      <td>None</td>\n",
              "      <td>9.4</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>None</td>\n",
              "      <td>1481061</td>\n",
              "      <td>[]</td>\n",
              "      <td>CHEMBL830868</td>\n",
              "      <td>In vitro inhibitory concentration against SARS...</td>\n",
              "      <td>B</td>\n",
              "      <td>BAO_0000190</td>\n",
              "      <td>BAO_0000357</td>\n",
              "      <td>single protein format</td>\n",
              "      <td>O=C1C(=O)N(CC2COc3ccccc3O2)c2ccc(I)cc21</td>\n",
              "      <td>None</td>\n",
              "      <td>None</td>\n",
              "      <td>CHEMBL1139624</td>\n",
              "      <td>Bioorg. Med. Chem. Lett.</td>\n",
              "      <td>2005</td>\n",
              "      <td>{'bei': '11.56', 'le': '0.29', 'lle': '2.21', ...</td>\n",
              "      <td>CHEMBL185698</td>\n",
              "      <td>None</td>\n",
              "      <td>CHEMBL185698</td>\n",
              "      <td>4.87</td>\n",
              "      <td>False</td>\n",
              "      <td>http://www.openphacts.org/units/Nanomolar</td>\n",
              "      <td>384106</td>\n",
              "      <td>=</td>\n",
              "      <td>1</td>\n",
              "      <td>True</td>\n",
              "      <td>=</td>\n",
              "      <td>None</td>\n",
              "      <td>IC50</td>\n",
              "      <td>nM</td>\n",
              "      <td>None</td>\n",
              "      <td>13500.0</td>\n",
              "      <td>CHEMBL3927</td>\n",
              "      <td>SARS coronavirus</td>\n",
              "      <td>SARS coronavirus 3C-like proteinase</td>\n",
              "      <td>227859</td>\n",
              "      <td>None</td>\n",
              "      <td>None</td>\n",
              "      <td>IC50</td>\n",
              "      <td>uM</td>\n",
              "      <td>UO_0000065</td>\n",
              "      <td>None</td>\n",
              "      <td>13.5</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>"
            ],
            "text/plain": [
              "  activity_comment  activity_id  ... upper_value value\n",
              "0             None      1480935  ...        None   7.2\n",
              "1             None      1480936  ...        None   9.4\n",
              "2             None      1481061  ...        None  13.5\n",
              "\n",
              "[3 rows x 43 columns]"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 8
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "oNtBv36dYhxy",
        "colab_type": "code",
        "outputId": "db6a7832-55eb-484c-b56c-98cdcd5944dd",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 35
        }
      },
      "source": [
        "df.standard_type.unique()"
      ],
      "execution_count": 0,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "array(['IC50'], dtype=object)"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 11
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "fQ78N26Fg15T",
        "colab_type": "text"
      },
      "source": [
        "Finally we will save the resulting bioactivity data to a CSV file **bioactivity_data.csv**."
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "ZvUUEIVxTOH1",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "df.to_csv('bioactivity_data.csv', index=False)"
      ],
      "execution_count": 0,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "BOrSrTGjOWU7",
        "colab_type": "text"
      },
      "source": [
        "## **Copying files to Google Drive**"
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "PRputWaI7ZW7",
        "colab_type": "text"
      },
      "source": [
        "Firstly, we need to mount the Google Drive into Colab so that we can have access to our Google adrive from within Colab."
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "6RBX658q65A5",
        "colab_type": "code",
        "outputId": "04a014cd-9f34-4a8f-e45f-50b380d9d41b",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 124
        }
      },
      "source": [
        "from google.colab import drive\n",
        "drive.mount('/content/gdrive/', force_remount=True)\n"
      ],
      "execution_count": 0,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "Go to this URL in a browser: https://accounts.google.com/o/oauth2/auth?client_id=947318989803-6bn6qk8qdgf4n4g3pfee6491hc0brc4i.apps.googleusercontent.com&redirect_uri=urn%3aietf%3awg%3aoauth%3a2.0%3aoob&response_type=code&scope=email%20https%3a%2f%2fwww.googleapis.com%2fauth%2fdocs.test%20https%3a%2f%2fwww.googleapis.com%2fauth%2fdrive%20https%3a%2f%2fwww.googleapis.com%2fauth%2fdrive.photos.readonly%20https%3a%2f%2fwww.googleapis.com%2fauth%2fpeopleapi.readonly\n",
            "\n",
            "Enter your authorization code:\n",
            "··········\n",
            "Mounted at /content/gdrive/\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "CMlY0xudN1mL",
        "colab_type": "text"
      },
      "source": [
        "Next, we create a **data** folder in our **Colab Notebooks** folder on Google Drive."
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "tew-UtUWIS__",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "! mkdir \"/content/gdrive/My Drive/Colab Notebooks/data2\""
      ],
      "execution_count": 0,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "YDMBpK2XJ_rJ",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "! cp bioactivity_data.csv \"/content/gdrive/My Drive/Colab Notebooks/data\""
      ],
      "execution_count": 0,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "iRIr1QiEJtuw",
        "colab_type": "code",
        "outputId": "e400f4d9-3ce7-4822-8837-33eb2499c1c1",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 52
        }
      },
      "source": [
        "! ls -l \"/content/gdrive/My Drive/Colab Notebooks/data\""
      ],
      "execution_count": 0,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "total 69\n",
            "-rw------- 1 root root 70010 Apr 29 17:10 bioactivity_data.csv\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "z9NwrYJni8CH",
        "colab_type": "text"
      },
      "source": [
        "Let's see the CSV files that we have so far."
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "FO3cZC5vnCht",
        "colab_type": "code",
        "outputId": "f5e07f1f-7a24-4d8e-ca52-e5e36e4daea1",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 35
        }
      },
      "source": [
        "! ls"
      ],
      "execution_count": 0,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "bioactivity_data.csv  gdrive  sample_data\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "7UAasSu5jAeB",
        "colab_type": "text"
      },
      "source": [
        "Taking a glimpse of the **bioactivity_data.csv** file that we've just created."
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "jwEJjx5b5gAn",
        "colab_type": "code",
        "outputId": "69dce8c6-565d-4537-952e-b01da9f2fd83",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 211
        }
      },
      "source": [
        "! head bioactivity_data.csv"
      ],
      "execution_count": 0,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "activity_comment,activity_id,activity_properties,assay_chembl_id,assay_description,assay_type,bao_endpoint,bao_format,bao_label,canonical_smiles,data_validity_comment,data_validity_description,document_chembl_id,document_journal,document_year,ligand_efficiency,molecule_chembl_id,molecule_pref_name,parent_molecule_chembl_id,pchembl_value,potential_duplicate,qudt_units,record_id,relation,src_id,standard_flag,standard_relation,standard_text_value,standard_type,standard_units,standard_upper_value,standard_value,target_chembl_id,target_organism,target_pref_name,target_tax_id,text_value,toid,type,units,uo_units,upper_value,value\n",
            ",1480935,[],CHEMBL829584,In vitro inhibitory concentration against SARS coronavirus main protease (SARS CoV 3C-like protease),B,BAO_0000190,BAO_0000357,single protein format,Cc1noc(C)c1CN1C(=O)C(=O)c2cc(C#N)ccc21,,,CHEMBL1139624,Bioorg. Med. Chem. Lett.,2005,\"{'bei': '18.28', 'le': '0.33', 'lle': '3.25', 'sei': '5.90'}\",CHEMBL187579,,CHEMBL187579,5.14,False,http://www.openphacts.org/units/Nanomolar,384103,=,1,True,=,,IC50,nM,,7200.0,CHEMBL3927,SARS coronavirus,SARS coronavirus 3C-like proteinase,227859,,,IC50,uM,UO_0000065,,7.2\n",
            ",1480936,[],CHEMBL829584,In vitro inhibitory concentration against SARS coronavirus main protease (SARS CoV 3C-like protease),B,BAO_0000190,BAO_0000357,single protein format,O=C1C(=O)N(Cc2ccc(F)cc2Cl)c2ccc(I)cc21,,,CHEMBL1139624,Bioorg. Med. Chem. Lett.,2005,\"{'bei': '12.10', 'le': '0.33', 'lle': '1.22', 'sei': '13.45'}\",CHEMBL188487,,CHEMBL188487,5.03,False,http://www.openphacts.org/units/Nanomolar,383984,=,1,True,=,,IC50,nM,,9400.0,CHEMBL3927,SARS coronavirus,SARS coronavirus 3C-like proteinase,227859,,,IC50,uM,UO_0000065,,9.4\n",
            ",1481061,[],CHEMBL830868,In vitro inhibitory concentration against SARS coronavirus main protease (SARS CoV 3C-like protease) at 20 uM,B,BAO_0000190,BAO_0000357,single protein format,O=C1C(=O)N(CC2COc3ccccc3O2)c2ccc(I)cc21,,,CHEMBL1139624,Bioorg. Med. Chem. Lett.,2005,\"{'bei': '11.56', 'le': '0.29', 'lle': '2.21', 'sei': '8.72'}\",CHEMBL185698,,CHEMBL185698,4.87,False,http://www.openphacts.org/units/Nanomolar,384106,=,1,True,=,,IC50,nM,,13500.0,CHEMBL3927,SARS coronavirus,SARS coronavirus 3C-like proteinase,227859,,,IC50,uM,UO_0000065,,13.5\n",
            ",1481065,[],CHEMBL829584,In vitro inhibitory concentration against SARS coronavirus main protease (SARS CoV 3C-like protease),B,BAO_0000190,BAO_0000357,single protein format,O=C1C(=O)N(Cc2cc3ccccc3s2)c2ccccc21,,,CHEMBL1139624,Bioorg. Med. Chem. Lett.,2005,\"{'bei': '16.64', 'le': '0.32', 'lle': '1.25', 'sei': '13.06'}\",CHEMBL426082,,CHEMBL426082,4.88,False,http://www.openphacts.org/units/Nanomolar,384075,=,1,True,=,,IC50,nM,,13110.0,CHEMBL3927,SARS coronavirus,SARS coronavirus 3C-like proteinase,227859,,,IC50,uM,UO_0000065,,13.11\n",
            ",1481066,[],CHEMBL829584,In vitro inhibitory concentration against SARS coronavirus main protease (SARS CoV 3C-like protease),B,BAO_0000190,BAO_0000357,single protein format,O=C1C(=O)N(Cc2cc3ccccc3s2)c2c1cccc2[N+](=O)[O-],,,CHEMBL1139624,Bioorg. Med. Chem. Lett.,2005,\"{'bei': '16.84', 'le': '0.32', 'lle': '2.16', 'sei': '7.08'}\",CHEMBL187717,,CHEMBL187717,5.70,False,http://www.openphacts.org/units/Nanomolar,384234,=,1,True,=,,IC50,nM,,2000.0,CHEMBL3927,SARS coronavirus,SARS coronavirus 3C-like proteinase,227859,,,IC50,uM,UO_0000065,,2.0\n",
            ",1481068,[],CHEMBL828143,In vitro inhibitory concentration SARS coronavirus main protease (SARS CoV 3C-like protease) ,B,BAO_0000190,BAO_0000357,single protein format,O=C1C(=O)N(Cc2cc3ccccc3s2)c2c(Br)cccc21,,,CHEMBL1139624,Bioorg. Med. Chem. Lett.,2005,\"{'bei': '16.14', 'le': '0.37', 'lle': '1.62', 'sei': '16.07'}\",CHEMBL365134,,CHEMBL365134,6.01,False,http://www.openphacts.org/units/Nanomolar,384081,=,1,True,=,,IC50,nM,,980.0,CHEMBL3927,SARS coronavirus,SARS coronavirus 3C-like proteinase,227859,,,IC50,uM,UO_0000065,,0.98\n",
            ",1481088,[],CHEMBL829584,In vitro inhibitory concentration against SARS coronavirus main protease (SARS CoV 3C-like protease),B,BAO_0000190,BAO_0000357,single protein format,O=C1C(=O)N(Cc2cc3ccccc3s2)c2ccc(F)cc21,,,CHEMBL1139624,Bioorg. Med. Chem. Lett.,2005,\"{'bei': '17.08', 'le': '0.33', 'lle': '1.55', 'sei': '14.22'}\",CHEMBL187598,,CHEMBL187598,5.32,False,http://www.openphacts.org/units/Nanomolar,384303,=,1,True,=,,IC50,nM,,4820.0,CHEMBL3927,SARS coronavirus,SARS coronavirus 3C-like proteinase,227859,,,IC50,uM,UO_0000065,,4.82\n",
            ",1481089,[],CHEMBL829584,In vitro inhibitory concentration against SARS coronavirus main protease (SARS CoV 3C-like protease),B,BAO_0000190,BAO_0000357,single protein format,O=C1C(=O)N(Cc2cc3ccccc3s2)c2ccc(I)cc21,,,CHEMBL1139624,Bioorg. Med. Chem. Lett.,2005,\"{'bei': '14.36', 'le': '0.37', 'lle': '1.78', 'sei': '16.11'}\",CHEMBL190743,,CHEMBL190743,6.02,False,http://www.openphacts.org/units/Nanomolar,384329,=,1,True,=,,IC50,nM,,950.0,CHEMBL3927,SARS coronavirus,SARS coronavirus 3C-like proteinase,227859,,,IC50,uM,UO_0000065,,0.95\n",
            ",1481093,[],CHEMBL829584,In vitro inhibitory concentration against SARS coronavirus main protease (SARS CoV 3C-like protease),B,BAO_0000190,BAO_0000357,single protein format,O=C1C(=O)N(Cc2cc3ccccc3s2)c2cccc(Cl)c21,,,CHEMBL1139624,Bioorg. Med. Chem. Lett.,2005,\"{'bei': '15.10', 'le': '0.31', 'lle': '0.67', 'sei': '13.24'}\",CHEMBL365469,,CHEMBL365469,4.95,False,http://www.openphacts.org/units/Nanomolar,384283,=,1,True,=,,IC50,nM,,11200.0,CHEMBL3927,SARS coronavirus,SARS coronavirus 3C-like proteinase,227859,,,IC50,uM,UO_0000065,,11.2\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "_GXMpFNUOn_8",
        "colab_type": "text"
      },
      "source": [
        "## **Handling missing data**\n",
        "If any compounds has missing value for the **standard_value** column then drop it"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "hkVOdk6ZR396",
        "colab_type": "code",
        "outputId": "fc08d57e-f832-4cb0-90f2-dc7394b0209d",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 782
        }
      },
      "source": [
        "df2 = df[df.standard_value.notna()]\n",
        "df2"
      ],
      "execution_count": 0,
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
              "      <th>activity_comment</th>\n",
              "      <th>activity_id</th>\n",
              "      <th>activity_properties</th>\n",
              "      <th>assay_chembl_id</th>\n",
              "      <th>assay_description</th>\n",
              "      <th>assay_type</th>\n",
              "      <th>bao_endpoint</th>\n",
              "      <th>bao_format</th>\n",
              "      <th>bao_label</th>\n",
              "      <th>canonical_smiles</th>\n",
              "      <th>data_validity_comment</th>\n",
              "      <th>data_validity_description</th>\n",
              "      <th>document_chembl_id</th>\n",
              "      <th>document_journal</th>\n",
              "      <th>document_year</th>\n",
              "      <th>ligand_efficiency</th>\n",
              "      <th>molecule_chembl_id</th>\n",
              "      <th>molecule_pref_name</th>\n",
              "      <th>parent_molecule_chembl_id</th>\n",
              "      <th>pchembl_value</th>\n",
              "      <th>potential_duplicate</th>\n",
              "      <th>qudt_units</th>\n",
              "      <th>record_id</th>\n",
              "      <th>relation</th>\n",
              "      <th>src_id</th>\n",
              "      <th>standard_flag</th>\n",
              "      <th>standard_relation</th>\n",
              "      <th>standard_text_value</th>\n",
              "      <th>standard_type</th>\n",
              "      <th>standard_units</th>\n",
              "      <th>standard_upper_value</th>\n",
              "      <th>standard_value</th>\n",
              "      <th>target_chembl_id</th>\n",
              "      <th>target_organism</th>\n",
              "      <th>target_pref_name</th>\n",
              "      <th>target_tax_id</th>\n",
              "      <th>text_value</th>\n",
              "      <th>toid</th>\n",
              "      <th>type</th>\n",
              "      <th>units</th>\n",
              "      <th>uo_units</th>\n",
              "      <th>upper_value</th>\n",
              "      <th>value</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>None</td>\n",
              "      <td>1480935</td>\n",
              "      <td>[]</td>\n",
              "      <td>CHEMBL829584</td>\n",
              "      <td>In vitro inhibitory concentration against SARS...</td>\n",
              "      <td>B</td>\n",
              "      <td>BAO_0000190</td>\n",
              "      <td>BAO_0000357</td>\n",
              "      <td>single protein format</td>\n",
              "      <td>Cc1noc(C)c1CN1C(=O)C(=O)c2cc(C#N)ccc21</td>\n",
              "      <td>None</td>\n",
              "      <td>None</td>\n",
              "      <td>CHEMBL1139624</td>\n",
              "      <td>Bioorg. Med. Chem. Lett.</td>\n",
              "      <td>2005</td>\n",
              "      <td>{'bei': '18.28', 'le': '0.33', 'lle': '3.25', ...</td>\n",
              "      <td>CHEMBL187579</td>\n",
              "      <td>None</td>\n",
              "      <td>CHEMBL187579</td>\n",
              "      <td>5.14</td>\n",
              "      <td>False</td>\n",
              "      <td>http://www.openphacts.org/units/Nanomolar</td>\n",
              "      <td>384103</td>\n",
              "      <td>=</td>\n",
              "      <td>1</td>\n",
              "      <td>True</td>\n",
              "      <td>=</td>\n",
              "      <td>None</td>\n",
              "      <td>IC50</td>\n",
              "      <td>nM</td>\n",
              "      <td>None</td>\n",
              "      <td>7200.0</td>\n",
              "      <td>CHEMBL3927</td>\n",
              "      <td>SARS coronavirus</td>\n",
              "      <td>SARS coronavirus 3C-like proteinase</td>\n",
              "      <td>227859</td>\n",
              "      <td>None</td>\n",
              "      <td>None</td>\n",
              "      <td>IC50</td>\n",
              "      <td>uM</td>\n",
              "      <td>UO_0000065</td>\n",
              "      <td>None</td>\n",
              "      <td>7.2</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>None</td>\n",
              "      <td>1480936</td>\n",
              "      <td>[]</td>\n",
              "      <td>CHEMBL829584</td>\n",
              "      <td>In vitro inhibitory concentration against SARS...</td>\n",
              "      <td>B</td>\n",
              "      <td>BAO_0000190</td>\n",
              "      <td>BAO_0000357</td>\n",
              "      <td>single protein format</td>\n",
              "      <td>O=C1C(=O)N(Cc2ccc(F)cc2Cl)c2ccc(I)cc21</td>\n",
              "      <td>None</td>\n",
              "      <td>None</td>\n",
              "      <td>CHEMBL1139624</td>\n",
              "      <td>Bioorg. Med. Chem. Lett.</td>\n",
              "      <td>2005</td>\n",
              "      <td>{'bei': '12.10', 'le': '0.33', 'lle': '1.22', ...</td>\n",
              "      <td>CHEMBL188487</td>\n",
              "      <td>None</td>\n",
              "      <td>CHEMBL188487</td>\n",
              "      <td>5.03</td>\n",
              "      <td>False</td>\n",
              "      <td>http://www.openphacts.org/units/Nanomolar</td>\n",
              "      <td>383984</td>\n",
              "      <td>=</td>\n",
              "      <td>1</td>\n",
              "      <td>True</td>\n",
              "      <td>=</td>\n",
              "      <td>None</td>\n",
              "      <td>IC50</td>\n",
              "      <td>nM</td>\n",
              "      <td>None</td>\n",
              "      <td>9400.0</td>\n",
              "      <td>CHEMBL3927</td>\n",
              "      <td>SARS coronavirus</td>\n",
              "      <td>SARS coronavirus 3C-like proteinase</td>\n",
              "      <td>227859</td>\n",
              "      <td>None</td>\n",
              "      <td>None</td>\n",
              "      <td>IC50</td>\n",
              "      <td>uM</td>\n",
              "      <td>UO_0000065</td>\n",
              "      <td>None</td>\n",
              "      <td>9.4</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>None</td>\n",
              "      <td>1481061</td>\n",
              "      <td>[]</td>\n",
              "      <td>CHEMBL830868</td>\n",
              "      <td>In vitro inhibitory concentration against SARS...</td>\n",
              "      <td>B</td>\n",
              "      <td>BAO_0000190</td>\n",
              "      <td>BAO_0000357</td>\n",
              "      <td>single protein format</td>\n",
              "      <td>O=C1C(=O)N(CC2COc3ccccc3O2)c2ccc(I)cc21</td>\n",
              "      <td>None</td>\n",
              "      <td>None</td>\n",
              "      <td>CHEMBL1139624</td>\n",
              "      <td>Bioorg. Med. Chem. Lett.</td>\n",
              "      <td>2005</td>\n",
              "      <td>{'bei': '11.56', 'le': '0.29', 'lle': '2.21', ...</td>\n",
              "      <td>CHEMBL185698</td>\n",
              "      <td>None</td>\n",
              "      <td>CHEMBL185698</td>\n",
              "      <td>4.87</td>\n",
              "      <td>False</td>\n",
              "      <td>http://www.openphacts.org/units/Nanomolar</td>\n",
              "      <td>384106</td>\n",
              "      <td>=</td>\n",
              "      <td>1</td>\n",
              "      <td>True</td>\n",
              "      <td>=</td>\n",
              "      <td>None</td>\n",
              "      <td>IC50</td>\n",
              "      <td>nM</td>\n",
              "      <td>None</td>\n",
              "      <td>13500.0</td>\n",
              "      <td>CHEMBL3927</td>\n",
              "      <td>SARS coronavirus</td>\n",
              "      <td>SARS coronavirus 3C-like proteinase</td>\n",
              "      <td>227859</td>\n",
              "      <td>None</td>\n",
              "      <td>None</td>\n",
              "      <td>IC50</td>\n",
              "      <td>uM</td>\n",
              "      <td>UO_0000065</td>\n",
              "      <td>None</td>\n",
              "      <td>13.5</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>None</td>\n",
              "      <td>1481065</td>\n",
              "      <td>[]</td>\n",
              "      <td>CHEMBL829584</td>\n",
              "      <td>In vitro inhibitory concentration against SARS...</td>\n",
              "      <td>B</td>\n",
              "      <td>BAO_0000190</td>\n",
              "      <td>BAO_0000357</td>\n",
              "      <td>single protein format</td>\n",
              "      <td>O=C1C(=O)N(Cc2cc3ccccc3s2)c2ccccc21</td>\n",
              "      <td>None</td>\n",
              "      <td>None</td>\n",
              "      <td>CHEMBL1139624</td>\n",
              "      <td>Bioorg. Med. Chem. Lett.</td>\n",
              "      <td>2005</td>\n",
              "      <td>{'bei': '16.64', 'le': '0.32', 'lle': '1.25', ...</td>\n",
              "      <td>CHEMBL426082</td>\n",
              "      <td>None</td>\n",
              "      <td>CHEMBL426082</td>\n",
              "      <td>4.88</td>\n",
              "      <td>False</td>\n",
              "      <td>http://www.openphacts.org/units/Nanomolar</td>\n",
              "      <td>384075</td>\n",
              "      <td>=</td>\n",
              "      <td>1</td>\n",
              "      <td>True</td>\n",
              "      <td>=</td>\n",
              "      <td>None</td>\n",
              "      <td>IC50</td>\n",
              "      <td>nM</td>\n",
              "      <td>None</td>\n",
              "      <td>13110.0</td>\n",
              "      <td>CHEMBL3927</td>\n",
              "      <td>SARS coronavirus</td>\n",
              "      <td>SARS coronavirus 3C-like proteinase</td>\n",
              "      <td>227859</td>\n",
              "      <td>None</td>\n",
              "      <td>None</td>\n",
              "      <td>IC50</td>\n",
              "      <td>uM</td>\n",
              "      <td>UO_0000065</td>\n",
              "      <td>None</td>\n",
              "      <td>13.11</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>None</td>\n",
              "      <td>1481066</td>\n",
              "      <td>[]</td>\n",
              "      <td>CHEMBL829584</td>\n",
              "      <td>In vitro inhibitory concentration against SARS...</td>\n",
              "      <td>B</td>\n",
              "      <td>BAO_0000190</td>\n",
              "      <td>BAO_0000357</td>\n",
              "      <td>single protein format</td>\n",
              "      <td>O=C1C(=O)N(Cc2cc3ccccc3s2)c2c1cccc2[N+](=O)[O-]</td>\n",
              "      <td>None</td>\n",
              "      <td>None</td>\n",
              "      <td>CHEMBL1139624</td>\n",
              "      <td>Bioorg. Med. Chem. Lett.</td>\n",
              "      <td>2005</td>\n",
              "      <td>{'bei': '16.84', 'le': '0.32', 'lle': '2.16', ...</td>\n",
              "      <td>CHEMBL187717</td>\n",
              "      <td>None</td>\n",
              "      <td>CHEMBL187717</td>\n",
              "      <td>5.70</td>\n",
              "      <td>False</td>\n",
              "      <td>http://www.openphacts.org/units/Nanomolar</td>\n",
              "      <td>384234</td>\n",
              "      <td>=</td>\n",
              "      <td>1</td>\n",
              "      <td>True</td>\n",
              "      <td>=</td>\n",
              "      <td>None</td>\n",
              "      <td>IC50</td>\n",
              "      <td>nM</td>\n",
              "      <td>None</td>\n",
              "      <td>2000.0</td>\n",
              "      <td>CHEMBL3927</td>\n",
              "      <td>SARS coronavirus</td>\n",
              "      <td>SARS coronavirus 3C-like proteinase</td>\n",
              "      <td>227859</td>\n",
              "      <td>None</td>\n",
              "      <td>None</td>\n",
              "      <td>IC50</td>\n",
              "      <td>uM</td>\n",
              "      <td>UO_0000065</td>\n",
              "      <td>None</td>\n",
              "      <td>2.0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>...</th>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>128</th>\n",
              "      <td>None</td>\n",
              "      <td>12041507</td>\n",
              "      <td>[]</td>\n",
              "      <td>CHEMBL2150313</td>\n",
              "      <td>Inhibition of SARS-CoV PLpro expressed in Esch...</td>\n",
              "      <td>B</td>\n",
              "      <td>BAO_0000190</td>\n",
              "      <td>BAO_0000019</td>\n",
              "      <td>assay format</td>\n",
              "      <td>COC(=O)[C@@]1(C)CCCc2c1ccc1c2C(=O)C(=O)c2c(C)c...</td>\n",
              "      <td>None</td>\n",
              "      <td>None</td>\n",
              "      <td>CHEMBL2146458</td>\n",
              "      <td>Bioorg. Med. Chem.</td>\n",
              "      <td>2012</td>\n",
              "      <td>{'bei': '14.70', 'le': '0.27', 'lle': '1.57', ...</td>\n",
              "      <td>CHEMBL2146517</td>\n",
              "      <td>METHYL TANSHINONATE</td>\n",
              "      <td>CHEMBL2146517</td>\n",
              "      <td>4.97</td>\n",
              "      <td>False</td>\n",
              "      <td>http://www.openphacts.org/units/Nanomolar</td>\n",
              "      <td>1727226</td>\n",
              "      <td>=</td>\n",
              "      <td>1</td>\n",
              "      <td>True</td>\n",
              "      <td>=</td>\n",
              "      <td>None</td>\n",
              "      <td>IC50</td>\n",
              "      <td>nM</td>\n",
              "      <td>None</td>\n",
              "      <td>10600.0</td>\n",
              "      <td>CHEMBL3927</td>\n",
              "      <td>SARS coronavirus</td>\n",
              "      <td>SARS coronavirus 3C-like proteinase</td>\n",
              "      <td>227859</td>\n",
              "      <td>None</td>\n",
              "      <td>None</td>\n",
              "      <td>IC50</td>\n",
              "      <td>uM</td>\n",
              "      <td>UO_0000065</td>\n",
              "      <td>None</td>\n",
              "      <td>10.6</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>129</th>\n",
              "      <td>None</td>\n",
              "      <td>12041508</td>\n",
              "      <td>[]</td>\n",
              "      <td>CHEMBL2150313</td>\n",
              "      <td>Inhibition of SARS-CoV PLpro expressed in Esch...</td>\n",
              "      <td>B</td>\n",
              "      <td>BAO_0000190</td>\n",
              "      <td>BAO_0000019</td>\n",
              "      <td>assay format</td>\n",
              "      <td>C[C@H]1COC2=C1C(=O)C(=O)c1c2ccc2c1CCCC2(C)C</td>\n",
              "      <td>None</td>\n",
              "      <td>None</td>\n",
              "      <td>CHEMBL2146458</td>\n",
              "      <td>Bioorg. Med. Chem.</td>\n",
              "      <td>2012</td>\n",
              "      <td>{'bei': '16.86', 'le': '0.31', 'lle': '1.56', ...</td>\n",
              "      <td>CHEMBL187460</td>\n",
              "      <td>CRYPTOTANSHINONE</td>\n",
              "      <td>CHEMBL187460</td>\n",
              "      <td>5.00</td>\n",
              "      <td>False</td>\n",
              "      <td>http://www.openphacts.org/units/Nanomolar</td>\n",
              "      <td>1727227</td>\n",
              "      <td>=</td>\n",
              "      <td>1</td>\n",
              "      <td>True</td>\n",
              "      <td>=</td>\n",
              "      <td>None</td>\n",
              "      <td>IC50</td>\n",
              "      <td>nM</td>\n",
              "      <td>None</td>\n",
              "      <td>10100.0</td>\n",
              "      <td>CHEMBL3927</td>\n",
              "      <td>SARS coronavirus</td>\n",
              "      <td>SARS coronavirus 3C-like proteinase</td>\n",
              "      <td>227859</td>\n",
              "      <td>None</td>\n",
              "      <td>None</td>\n",
              "      <td>IC50</td>\n",
              "      <td>uM</td>\n",
              "      <td>UO_0000065</td>\n",
              "      <td>None</td>\n",
              "      <td>10.1</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>130</th>\n",
              "      <td>None</td>\n",
              "      <td>12041509</td>\n",
              "      <td>[]</td>\n",
              "      <td>CHEMBL2150313</td>\n",
              "      <td>Inhibition of SARS-CoV PLpro expressed in Esch...</td>\n",
              "      <td>B</td>\n",
              "      <td>BAO_0000190</td>\n",
              "      <td>BAO_0000019</td>\n",
              "      <td>assay format</td>\n",
              "      <td>Cc1coc2c1C(=O)C(=O)c1c-2ccc2c(C)cccc12</td>\n",
              "      <td>None</td>\n",
              "      <td>None</td>\n",
              "      <td>CHEMBL2146458</td>\n",
              "      <td>Bioorg. Med. Chem.</td>\n",
              "      <td>2012</td>\n",
              "      <td>{'bei': '17.88', 'le': '0.32', 'lle': '0.84', ...</td>\n",
              "      <td>CHEMBL363535</td>\n",
              "      <td>TANSHINONE I</td>\n",
              "      <td>CHEMBL363535</td>\n",
              "      <td>4.94</td>\n",
              "      <td>False</td>\n",
              "      <td>http://www.openphacts.org/units/Nanomolar</td>\n",
              "      <td>1727228</td>\n",
              "      <td>=</td>\n",
              "      <td>1</td>\n",
              "      <td>True</td>\n",
              "      <td>=</td>\n",
              "      <td>None</td>\n",
              "      <td>IC50</td>\n",
              "      <td>nM</td>\n",
              "      <td>None</td>\n",
              "      <td>11500.0</td>\n",
              "      <td>CHEMBL3927</td>\n",
              "      <td>SARS coronavirus</td>\n",
              "      <td>SARS coronavirus 3C-like proteinase</td>\n",
              "      <td>227859</td>\n",
              "      <td>None</td>\n",
              "      <td>None</td>\n",
              "      <td>IC50</td>\n",
              "      <td>uM</td>\n",
              "      <td>UO_0000065</td>\n",
              "      <td>None</td>\n",
              "      <td>11.5</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>131</th>\n",
              "      <td>None</td>\n",
              "      <td>12041510</td>\n",
              "      <td>[]</td>\n",
              "      <td>CHEMBL2150313</td>\n",
              "      <td>Inhibition of SARS-CoV PLpro expressed in Esch...</td>\n",
              "      <td>B</td>\n",
              "      <td>BAO_0000190</td>\n",
              "      <td>BAO_0000019</td>\n",
              "      <td>assay format</td>\n",
              "      <td>Cc1cccc2c3c(ccc12)C1=C(C(=O)C3=O)[C@@H](C)CO1</td>\n",
              "      <td>None</td>\n",
              "      <td>None</td>\n",
              "      <td>CHEMBL2146458</td>\n",
              "      <td>Bioorg. Med. Chem.</td>\n",
              "      <td>2012</td>\n",
              "      <td>{'bei': '17.86', 'le': '0.32', 'lle': '1.68', ...</td>\n",
              "      <td>CHEMBL227075</td>\n",
              "      <td>DIHYDROTANSHINONE I</td>\n",
              "      <td>CHEMBL227075</td>\n",
              "      <td>4.97</td>\n",
              "      <td>False</td>\n",
              "      <td>http://www.openphacts.org/units/Nanomolar</td>\n",
              "      <td>1727229</td>\n",
              "      <td>=</td>\n",
              "      <td>1</td>\n",
              "      <td>True</td>\n",
              "      <td>=</td>\n",
              "      <td>None</td>\n",
              "      <td>IC50</td>\n",
              "      <td>nM</td>\n",
              "      <td>None</td>\n",
              "      <td>10700.0</td>\n",
              "      <td>CHEMBL3927</td>\n",
              "      <td>SARS coronavirus</td>\n",
              "      <td>SARS coronavirus 3C-like proteinase</td>\n",
              "      <td>227859</td>\n",
              "      <td>None</td>\n",
              "      <td>None</td>\n",
              "      <td>IC50</td>\n",
              "      <td>uM</td>\n",
              "      <td>UO_0000065</td>\n",
              "      <td>None</td>\n",
              "      <td>10.7</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>132</th>\n",
              "      <td>None</td>\n",
              "      <td>12041511</td>\n",
              "      <td>[]</td>\n",
              "      <td>CHEMBL2150313</td>\n",
              "      <td>Inhibition of SARS-CoV PLpro expressed in Esch...</td>\n",
              "      <td>B</td>\n",
              "      <td>BAO_0000190</td>\n",
              "      <td>BAO_0000019</td>\n",
              "      <td>assay format</td>\n",
              "      <td>CC(C)C1=Cc2ccc3c(c2C(=O)C1=O)CCCC3(C)C</td>\n",
              "      <td>None</td>\n",
              "      <td>None</td>\n",
              "      <td>CHEMBL2146458</td>\n",
              "      <td>Bioorg. Med. Chem.</td>\n",
              "      <td>2012</td>\n",
              "      <td>{'bei': '14.53', 'le': '0.27', 'lle': '-0.01',...</td>\n",
              "      <td>CHEMBL45830</td>\n",
              "      <td>MILTIRONE</td>\n",
              "      <td>CHEMBL45830</td>\n",
              "      <td>4.10</td>\n",
              "      <td>False</td>\n",
              "      <td>http://www.openphacts.org/units/Nanomolar</td>\n",
              "      <td>1727230</td>\n",
              "      <td>=</td>\n",
              "      <td>1</td>\n",
              "      <td>True</td>\n",
              "      <td>=</td>\n",
              "      <td>None</td>\n",
              "      <td>IC50</td>\n",
              "      <td>nM</td>\n",
              "      <td>None</td>\n",
              "      <td>78900.0</td>\n",
              "      <td>CHEMBL3927</td>\n",
              "      <td>SARS coronavirus</td>\n",
              "      <td>SARS coronavirus 3C-like proteinase</td>\n",
              "      <td>227859</td>\n",
              "      <td>None</td>\n",
              "      <td>None</td>\n",
              "      <td>IC50</td>\n",
              "      <td>uM</td>\n",
              "      <td>UO_0000065</td>\n",
              "      <td>None</td>\n",
              "      <td>78.9</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "<p>133 rows × 43 columns</p>\n",
              "</div>"
            ],
            "text/plain": [
              "    activity_comment  activity_id  ... upper_value  value\n",
              "0               None      1480935  ...        None    7.2\n",
              "1               None      1480936  ...        None    9.4\n",
              "2               None      1481061  ...        None   13.5\n",
              "3               None      1481065  ...        None  13.11\n",
              "4               None      1481066  ...        None    2.0\n",
              "..               ...          ...  ...         ...    ...\n",
              "128             None     12041507  ...        None   10.6\n",
              "129             None     12041508  ...        None   10.1\n",
              "130             None     12041509  ...        None   11.5\n",
              "131             None     12041510  ...        None   10.7\n",
              "132             None     12041511  ...        None   78.9\n",
              "\n",
              "[133 rows x 43 columns]"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 23
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "Y-qNsUlmjS25",
        "colab_type": "text"
      },
      "source": [
        "Apparently, for this dataset there is no missing data. But we can use the above code cell for bioactivity data of other target protein."
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "5H4sSFAWhV9B",
        "colab_type": "text"
      },
      "source": [
        "## **Data pre-processing of the bioactivity data**"
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "tO22XVlzhkXR",
        "colab_type": "text"
      },
      "source": [
        "### **Labeling compounds as either being active, inactive or intermediate**\n",
        "The bioactivity data is in the IC50 unit. Compounds having values of less than 1000 nM will be considered to be **active** while those greater than 10,000 nM will be considered to be **inactive**. As for those values in between 1,000 and 10,000 nM will be referred to as **intermediate**. "
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "1E8rz7oMOd-5",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "bioactivity_class = []\n",
        "for i in df2.standard_value:\n",
        "  if float(i) >= 10000:\n",
        "    bioactivity_class.append(\"inactive\")\n",
        "  elif float(i) <= 1000:\n",
        "    bioactivity_class.append(\"active\")\n",
        "  else:\n",
        "    bioactivity_class.append(\"intermediate\")"
      ],
      "execution_count": 0,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "PFsmb2N9hnTB",
        "colab_type": "text"
      },
      "source": [
        "### **Iterate the *molecule_chembl_id* to a list**"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "DMJng9xnVnMM",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "mol_cid = []\n",
        "for i in df2.molecule_chembl_id:\n",
        "  mol_cid.append(i)"
      ],
      "execution_count": 0,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "YRieJc9dhuVZ",
        "colab_type": "text"
      },
      "source": [
        "### **Iterate *canonical_smiles* to a list**"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "AT8qUBk1eVmj",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "canonical_smiles = []\n",
        "for i in df2.canonical_smiles:\n",
        "  canonical_smiles.append(i)"
      ],
      "execution_count": 0,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "DZFugUXxhwjE",
        "colab_type": "text"
      },
      "source": [
        "### **Iterate *standard_value* to a list**"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "ZaPt-FjEZNBe",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "standard_value = []\n",
        "for i in df2.standard_value:\n",
        "  standard_value.append(i)"
      ],
      "execution_count": 0,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "Nv2dzid_hzKd",
        "colab_type": "text"
      },
      "source": [
        "### **Combine the 4 lists into a dataframe**"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "TWlYO4I3Wrh-",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "data_tuples = list(zip(mol_cid, canonical_smiles, bioactivity_class, standard_value))\n",
        "df3 = pd.DataFrame( data_tuples,  columns=['molecule_chembl_id', 'canonical_smiles', 'bioactivity_class', 'standard_value'])"
      ],
      "execution_count": 0,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "Li64nUiZQ-y2",
        "colab_type": "code",
        "outputId": "a1d4cdb5-922d-4573-9f8b-abcb7ef72a58",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 415
        }
      },
      "source": [
        "df3"
      ],
      "execution_count": 0,
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
              "      <th>molecule_chembl_id</th>\n",
              "      <th>canonical_smiles</th>\n",
              "      <th>bioactivity_class</th>\n",
              "      <th>standard_value</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>CHEMBL187579</td>\n",
              "      <td>Cc1noc(C)c1CN1C(=O)C(=O)c2cc(C#N)ccc21</td>\n",
              "      <td>intermediate</td>\n",
              "      <td>7200.0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>CHEMBL188487</td>\n",
              "      <td>O=C1C(=O)N(Cc2ccc(F)cc2Cl)c2ccc(I)cc21</td>\n",
              "      <td>intermediate</td>\n",
              "      <td>9400.0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>CHEMBL185698</td>\n",
              "      <td>O=C1C(=O)N(CC2COc3ccccc3O2)c2ccc(I)cc21</td>\n",
              "      <td>inactive</td>\n",
              "      <td>13500.0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>CHEMBL426082</td>\n",
              "      <td>O=C1C(=O)N(Cc2cc3ccccc3s2)c2ccccc21</td>\n",
              "      <td>inactive</td>\n",
              "      <td>13110.0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>CHEMBL187717</td>\n",
              "      <td>O=C1C(=O)N(Cc2cc3ccccc3s2)c2c1cccc2[N+](=O)[O-]</td>\n",
              "      <td>intermediate</td>\n",
              "      <td>2000.0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>...</th>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>128</th>\n",
              "      <td>CHEMBL2146517</td>\n",
              "      <td>COC(=O)[C@@]1(C)CCCc2c1ccc1c2C(=O)C(=O)c2c(C)c...</td>\n",
              "      <td>inactive</td>\n",
              "      <td>10600.0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>129</th>\n",
              "      <td>CHEMBL187460</td>\n",
              "      <td>C[C@H]1COC2=C1C(=O)C(=O)c1c2ccc2c1CCCC2(C)C</td>\n",
              "      <td>inactive</td>\n",
              "      <td>10100.0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>130</th>\n",
              "      <td>CHEMBL363535</td>\n",
              "      <td>Cc1coc2c1C(=O)C(=O)c1c-2ccc2c(C)cccc12</td>\n",
              "      <td>inactive</td>\n",
              "      <td>11500.0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>131</th>\n",
              "      <td>CHEMBL227075</td>\n",
              "      <td>Cc1cccc2c3c(ccc12)C1=C(C(=O)C3=O)[C@@H](C)CO1</td>\n",
              "      <td>inactive</td>\n",
              "      <td>10700.0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>132</th>\n",
              "      <td>CHEMBL45830</td>\n",
              "      <td>CC(C)C1=Cc2ccc3c(c2C(=O)C1=O)CCCC3(C)C</td>\n",
              "      <td>inactive</td>\n",
              "      <td>78900.0</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "<p>133 rows × 4 columns</p>\n",
              "</div>"
            ],
            "text/plain": [
              "    molecule_chembl_id  ... standard_value\n",
              "0         CHEMBL187579  ...         7200.0\n",
              "1         CHEMBL188487  ...         9400.0\n",
              "2         CHEMBL185698  ...        13500.0\n",
              "3         CHEMBL426082  ...        13110.0\n",
              "4         CHEMBL187717  ...         2000.0\n",
              "..                 ...  ...            ...\n",
              "128      CHEMBL2146517  ...        10600.0\n",
              "129       CHEMBL187460  ...        10100.0\n",
              "130       CHEMBL363535  ...        11500.0\n",
              "131       CHEMBL227075  ...        10700.0\n",
              "132        CHEMBL45830  ...        78900.0\n",
              "\n",
              "[133 rows x 4 columns]"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 34
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "vE0Vvo6ic3MI",
        "colab_type": "text"
      },
      "source": [
        "### **Alternative method**"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "VICiiCtqc2ne",
        "colab_type": "code",
        "outputId": "0b39e703-b724-4f02-d86c-ea07791cdeed",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 415
        }
      },
      "source": [
        "selection = ['molecule_chembl_id', 'canonical_smiles', 'standard_value']\n",
        "df3 = df2[selection]\n",
        "df3"
      ],
      "execution_count": 0,
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
              "      <th>molecule_chembl_id</th>\n",
              "      <th>canonical_smiles</th>\n",
              "      <th>standard_value</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>CHEMBL187579</td>\n",
              "      <td>Cc1noc(C)c1CN1C(=O)C(=O)c2cc(C#N)ccc21</td>\n",
              "      <td>7200.0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>CHEMBL188487</td>\n",
              "      <td>O=C1C(=O)N(Cc2ccc(F)cc2Cl)c2ccc(I)cc21</td>\n",
              "      <td>9400.0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>CHEMBL185698</td>\n",
              "      <td>O=C1C(=O)N(CC2COc3ccccc3O2)c2ccc(I)cc21</td>\n",
              "      <td>13500.0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>CHEMBL426082</td>\n",
              "      <td>O=C1C(=O)N(Cc2cc3ccccc3s2)c2ccccc21</td>\n",
              "      <td>13110.0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>CHEMBL187717</td>\n",
              "      <td>O=C1C(=O)N(Cc2cc3ccccc3s2)c2c1cccc2[N+](=O)[O-]</td>\n",
              "      <td>2000.0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>...</th>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>128</th>\n",
              "      <td>CHEMBL2146517</td>\n",
              "      <td>COC(=O)[C@@]1(C)CCCc2c1ccc1c2C(=O)C(=O)c2c(C)c...</td>\n",
              "      <td>10600.0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>129</th>\n",
              "      <td>CHEMBL187460</td>\n",
              "      <td>C[C@H]1COC2=C1C(=O)C(=O)c1c2ccc2c1CCCC2(C)C</td>\n",
              "      <td>10100.0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>130</th>\n",
              "      <td>CHEMBL363535</td>\n",
              "      <td>Cc1coc2c1C(=O)C(=O)c1c-2ccc2c(C)cccc12</td>\n",
              "      <td>11500.0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>131</th>\n",
              "      <td>CHEMBL227075</td>\n",
              "      <td>Cc1cccc2c3c(ccc12)C1=C(C(=O)C3=O)[C@@H](C)CO1</td>\n",
              "      <td>10700.0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>132</th>\n",
              "      <td>CHEMBL45830</td>\n",
              "      <td>CC(C)C1=Cc2ccc3c(c2C(=O)C1=O)CCCC3(C)C</td>\n",
              "      <td>78900.0</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "<p>133 rows × 3 columns</p>\n",
              "</div>"
            ],
            "text/plain": [
              "    molecule_chembl_id  ... standard_value\n",
              "0         CHEMBL187579  ...         7200.0\n",
              "1         CHEMBL188487  ...         9400.0\n",
              "2         CHEMBL185698  ...        13500.0\n",
              "3         CHEMBL426082  ...        13110.0\n",
              "4         CHEMBL187717  ...         2000.0\n",
              "..                 ...  ...            ...\n",
              "128      CHEMBL2146517  ...        10600.0\n",
              "129       CHEMBL187460  ...        10100.0\n",
              "130       CHEMBL363535  ...        11500.0\n",
              "131       CHEMBL227075  ...        10700.0\n",
              "132        CHEMBL45830  ...        78900.0\n",
              "\n",
              "[133 rows x 3 columns]"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 37
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "d8nV77oWdbq1",
        "colab_type": "code",
        "outputId": "2df59721-3567-48bc-a732-a0b09fa8aa12",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 415
        }
      },
      "source": [
        "pd.concat([df3,pd.Series(bioactivity_class)], axis=1)"
      ],
      "execution_count": 0,
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
              "      <th>molecule_chembl_id</th>\n",
              "      <th>canonical_smiles</th>\n",
              "      <th>bioactivity_class</th>\n",
              "      <th>standard_value</th>\n",
              "      <th>0</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>CHEMBL187579</td>\n",
              "      <td>Cc1noc(C)c1CN1C(=O)C(=O)c2cc(C#N)ccc21</td>\n",
              "      <td>intermediate</td>\n",
              "      <td>7200.0</td>\n",
              "      <td>intermediate</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>CHEMBL188487</td>\n",
              "      <td>O=C1C(=O)N(Cc2ccc(F)cc2Cl)c2ccc(I)cc21</td>\n",
              "      <td>intermediate</td>\n",
              "      <td>9400.0</td>\n",
              "      <td>intermediate</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>CHEMBL185698</td>\n",
              "      <td>O=C1C(=O)N(CC2COc3ccccc3O2)c2ccc(I)cc21</td>\n",
              "      <td>inactive</td>\n",
              "      <td>13500.0</td>\n",
              "      <td>inactive</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>CHEMBL426082</td>\n",
              "      <td>O=C1C(=O)N(Cc2cc3ccccc3s2)c2ccccc21</td>\n",
              "      <td>inactive</td>\n",
              "      <td>13110.0</td>\n",
              "      <td>inactive</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>CHEMBL187717</td>\n",
              "      <td>O=C1C(=O)N(Cc2cc3ccccc3s2)c2c1cccc2[N+](=O)[O-]</td>\n",
              "      <td>intermediate</td>\n",
              "      <td>2000.0</td>\n",
              "      <td>intermediate</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>...</th>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "      <td>...</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>128</th>\n",
              "      <td>CHEMBL2146517</td>\n",
              "      <td>COC(=O)[C@@]1(C)CCCc2c1ccc1c2C(=O)C(=O)c2c(C)c...</td>\n",
              "      <td>inactive</td>\n",
              "      <td>10600.0</td>\n",
              "      <td>inactive</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>129</th>\n",
              "      <td>CHEMBL187460</td>\n",
              "      <td>C[C@H]1COC2=C1C(=O)C(=O)c1c2ccc2c1CCCC2(C)C</td>\n",
              "      <td>inactive</td>\n",
              "      <td>10100.0</td>\n",
              "      <td>inactive</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>130</th>\n",
              "      <td>CHEMBL363535</td>\n",
              "      <td>Cc1coc2c1C(=O)C(=O)c1c-2ccc2c(C)cccc12</td>\n",
              "      <td>inactive</td>\n",
              "      <td>11500.0</td>\n",
              "      <td>inactive</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>131</th>\n",
              "      <td>CHEMBL227075</td>\n",
              "      <td>Cc1cccc2c3c(ccc12)C1=C(C(=O)C3=O)[C@@H](C)CO1</td>\n",
              "      <td>inactive</td>\n",
              "      <td>10700.0</td>\n",
              "      <td>inactive</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>132</th>\n",
              "      <td>CHEMBL45830</td>\n",
              "      <td>CC(C)C1=Cc2ccc3c(c2C(=O)C1=O)CCCC3(C)C</td>\n",
              "      <td>inactive</td>\n",
              "      <td>78900.0</td>\n",
              "      <td>inactive</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "<p>133 rows × 5 columns</p>\n",
              "</div>"
            ],
            "text/plain": [
              "    molecule_chembl_id  ...             0\n",
              "0         CHEMBL187579  ...  intermediate\n",
              "1         CHEMBL188487  ...  intermediate\n",
              "2         CHEMBL185698  ...      inactive\n",
              "3         CHEMBL426082  ...      inactive\n",
              "4         CHEMBL187717  ...  intermediate\n",
              "..                 ...  ...           ...\n",
              "128      CHEMBL2146517  ...      inactive\n",
              "129       CHEMBL187460  ...      inactive\n",
              "130       CHEMBL363535  ...      inactive\n",
              "131       CHEMBL227075  ...      inactive\n",
              "132        CHEMBL45830  ...      inactive\n",
              "\n",
              "[133 rows x 5 columns]"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 36
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "9tlgyexWh7YJ",
        "colab_type": "text"
      },
      "source": [
        "Saves dataframe to CSV file"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "nSNia7suXstR",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "df3.to_csv('bioactivity_preprocessed_data.csv', index=False)"
      ],
      "execution_count": 0,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "UuZf5-MEd-H5",
        "colab_type": "code",
        "outputId": "19e008f4-267b-490b-9b2c-e5f88a47a48a",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 104
        }
      },
      "source": [
        "! ls -l"
      ],
      "execution_count": 0,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "total 92\n",
            "-rw-r--r-- 1 root root 70010 Apr 29 17:07 bioactivity_data.csv\n",
            "-rw-r--r-- 1 root root  9326 Apr 29 17:24 bioactivity_preprocessed_data.csv\n",
            "drwx------ 4 root root  4096 Apr 29 17:08 gdrive\n",
            "drwxr-xr-x 1 root root  4096 Apr  3 16:24 sample_data\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "_C7rqJKTePhV",
        "colab_type": "text"
      },
      "source": [
        "Let's copy to the Google Drive"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "ZfyvJcENeHDB",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "! cp bioactivity_preprocessed_data.csv \"/content/gdrive/My Drive/Colab Notebooks/data\""
      ],
      "execution_count": 0,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "7PU7yU9leLV5",
        "colab_type": "code",
        "outputId": "c07ddf6b-e372-4807-bc0f-7f0b2944a0cf",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 35
        }
      },
      "source": [
        "! ls \"/content/gdrive/My Drive/Colab Notebooks/data\""
      ],
      "execution_count": 0,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "bioactivity_data.csv  bioactivity_preprocessed_data.csv\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "ZywB5K_Dlawb",
        "colab_type": "text"
      },
      "source": [
        "---"
      ]
    }
  ]
}
