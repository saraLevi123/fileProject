# fileProject
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.IO;
using System.Linq;
using System.Reflection;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace FormsProject
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void button1_Click(object sender, EventArgs e)
        {
            openFileDialog1.ShowDialog();
            if (openFileDialog1.ShowDialog() == DialogResult.OK)
            {
                string path = openFileDialog1.FileName;
                string file = File.ReadAllText(path);
                string[] lines = File.ReadAllLines(path);
                string[] words = file.Split(' ','\t', '\n','\r');
                words = words.Where(w => w != " ").ToArray();

                string[] wordsLow;
                wordsLow= Array.ConvertAll(words, d => d.ToLower());
                //מספר שורות
                txtCount.Text = lines.Length.ToString();
                //מספר מילים
                txtWordCount.Text = words.Length.ToString();
                //מספר מילים יחודיות
                uniqueWord(wordsLow);
                //אורך משפט ממוצע וארוך
                LengthSentence(words);
                //רצף ארוך ללא k
                withoutK(wordsLow);
                //צבעים בטופס
                getColor(words);
            }

        }
        private void uniqueWord(string[] words)
        {
            Dictionary<string, int> wordCount = new Dictionary<string, int>();
            foreach (string word in words)
            {
                    try
                    {
                        wordCount.Add(word, 1);
                    }
                    catch (Exception)
                    {
                        wordCount[word] += 1;
                    }

            }
            int countUnique = 0;
            foreach (var item in wordCount)
            {
                if (item.Value == 1)
                {
                    countUnique++;
                }
                txtUniqueWord.Text = countUnique.ToString();
            }

        }
        private void LengthSentence(string[] words)
        {
            List<int> LengthSentence = new List<int>();
            int countSentence = 0;
            foreach (string w in words)
            {
                if (w.Contains('.') || w.Contains('!') || w.Contains('?'))
                {
                    LengthSentence.Add(countSentence);
                    countSentence = 0;
                }
                else
                    countSentence++;
            }
            txtClassicSentence.Text = (LengthSentence[LengthSentence.Count / 2]).ToString();
            //אורך משפט ארוך
            txtLongSentence.Text = (LengthSentence[LengthSentence.Count - 1]).ToString();
        }
        private void withoutK(string[] words)
        {
            List<string> LengthSentence1 = new List<string>();
            List<string> currentSentence = new List<string>();

            foreach (string word in words)
            {
                if (word.Contains('k'))
                {
                    if (currentSentence.Count >= LengthSentence1.Count)
                    {
                        LengthSentence1.AddRange(currentSentence);
                        currentSentence.Clear();
                    }
                    break;
                }

                currentSentence.Add(word);
            }
            txtK.Text = " ";
            foreach (string ls in LengthSentence1)
            {
                txtK.Text += ls;
            }
        }
        private void getColor(string[] words)
        {
            KnownColor[] colors = (KnownColor[])Enum.GetValues(typeof(KnownColor));
            List<string> colorsString = new List<string>();
            foreach (KnownColor knowColor in colors)
            {
                string color = Color.FromKnownColor(knowColor).Name;
                color = color.ToLower();
                colorsString.Add(color);
            }

            Dictionary<string, int> colorCount = new Dictionary<string, int>();

            foreach (string w in words)
            {
                if (colorsString.Contains(w))
                {
                    try
                    {
                        colorCount.Add(w, 1);
                    }
                    catch (Exception)
                    {
                        colorCount[w] += 1;
                    }
                }
            }
            txtColor.Text = " ";
            foreach (var item in colorCount)
            {
                txtColor.Text += item.Key + " " + item.Value + ",";
            }
        }

        private void txtK_TextChanged(object sender, EventArgs e)
        {

        }
    }
}
