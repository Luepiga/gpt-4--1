About this Manual
This manual contains six beginner-friendly Python experiments that use a locally running Ollama
language model. The programs are command-line applications so that students can practise and
demonstrate them without Internet access.
Common Software Requirements
 Python 3.9 or later.
 Ollama installed and running on the laboratory computer.
 A model downloaded before the examination (for example, llama3.2). The exact model name must
match the name shown by the `ollama list` command.
 Python requests package.
One-time Setup
pip install requests
ollama list
ollama run llama3.2
The `ollama run` command is useful for checking that the model is available. After checking, exit the
model prompt. Ollama's local API is normally available at http://localhost:11434.
Common Helper Program
Save the following program as `ollama_helper.py` in the same folder as the six experiment programs.
All six experiments import this function.
import requests
def ask_ollama(prompt):
url = "http://localhost:11434/api/generate"
data = {
"model": "llama3.2",
"prompt": prompt,
"stream": False
}
response = requests.post(url, json=data, timeout=180)
response.raise_for_status()
return response.json()["response"]
Important: Replace `llama3.2` with the exact model name installed on the computer. Do not use a
model name that is not shown by `ollama list`.

25CS2506 – BUILDING APPLICATIONS USING GPT-4

General Instructions
 Start Ollama before running an experiment.
 Keep the helper file and experiment program in the same folder.
 Use the sample datasets.
 The programs use only the local Ollama service; Internet access is not required.
 Generated answers may vary between runs because they are produced by a language model.
 For the medical assistant, use only general educational information. The application must not
diagnose conditions or prescribe medicines.

Experiment 1 – Building an E-commerce Chatbot
Aim
To develop a simple e-commerce chatbot that answers customer questions using product information
and a local Ollama model.
Sample Question
A small online store wants a chatbot to answer customer questions about its products. Develop a
Python program that displays the available products, accepts customer questions, and uses Ollama to
answer using only the given product information. If the requested information is not available, the
chatbot should say so.
Sample Dataset
Product Price Description
Laptop 45000 8GB RAM, 512GB SSD
Mobile 15000 6GB RAM, 128GB storage
Headphones 2000 Wireless headphones
Keyboard 800 USB keyboard
Mouse 500 Wireless mouse

Save the table above as `products.csv` in the same folder as the Python program.
Python Program
from ollama_helper import ask_ollama
import csv
# Read product information from CSV
products = []
with open("products.csv", "r", encoding="utf-8") as file:
reader = csv.DictReader(file)
for row in reader:
products.append(row)
print("WELCOME TO ONLINE SHOPPING")

25CS2506 – BUILDING APPLICATIONS USING GPT-4

print("\nAvailable Products:")
for product in products:
print(product["Product"], "-", "Rs.", product["Price"],
"-", product["Description"])
while True:
question = input("\nAsk a question (type exit to stop): ")
if question.lower() == "exit":
break
prompt = f"""
You are an e-commerce chatbot.
Use only the product information below.
Do not invent products, prices or features.
If the information is not available, say so.
Product information: {products}
Customer question: {question}
"""
answer = ask_ollama(prompt)
print("\nChatbot:", answer)

Sample Input / Expected Output
Ask a question: Suggest a product below Rs. 1000.
Chatbot: The keyboard (Rs. 800) and wireless mouse (Rs. 500) are within your budget.
Result
The program is executed successfully and the required output is obtained.

25CS2506 – BUILDING APPLICATIONS USING GPT-4

Experiment 2 – AI-Powered Blog Content Generator
Aim
To generate blog content using a topic and writing style supplied by the user.
Sample Question
An educational website needs a simple blog generator. Develop a Python program that accepts a blog
topic and writing style, generates a suitable title and blog content using Ollama, displays the result, and
saves it in a text file.
Sample Dataset
No external dataset is required. The user provides the input while running the program.
Python Program
from ollama_helper import ask_ollama
print("AI BLOG CONTENT GENERATOR")
topic = input("Enter blog topic: ")
style = input("Enter writing style (Formal/Casual/Informative): ")
prompt = f"""
Write a blog on the topic: {topic}.
Use this writing style: {style}.
Include:
1. An attractive title
2. Introduction
3. Main content with headings
4. Conclusion
"""
blog = ask_ollama(prompt)
print("\nGENERATED BLOG\n")
print(blog)
with open("blog.txt", "w", encoding="utf-8") as file:
file.write(blog)
print("\nBlog saved as blog.txt")

Sample Input / Expected Output
Topic: Benefits of Artificial Intelligence
Style: Informative

25CS2506 – BUILDING APPLICATIONS USING GPT-4

Output: The model generates a title, introduction, main content and conclusion, and saves the result in
blog.txt.
Result
The program is executed successfully and the required output is obtained.

Experiment 3 – Intelligent Email Assistant
Aim
To generate an appropriate email reply based on the received email and the tone selected by the user.
Sample Question
A company wants to prepare email replies quickly. Develop a Python program that accepts an incoming
email and a preferred tone (Formal, Friendly or Professional). Use Ollama to generate a suitable
subject line and email reply, display it, and save it in a text file.
Sample Dataset
No external dataset is required. The user provides the input while running the program.
Python Program
from ollama_helper import ask_ollama
print("INTELLIGENT EMAIL ASSISTANT")
email = input("Enter the received email: ")
tone = input("Enter tone (Formal/Friendly/Professional): ")
prompt = f"""
You are an intelligent email assistant.
Read the received email and write an appropriate reply.
Received email:
{email}
Tone: {tone}
Include a suitable subject, greeting, email body and closing.
Do not invent facts that are not given in the email.
"""
reply = ask_ollama(prompt)
print("\nGENERATED EMAIL REPLY\n")
print(reply)
with open("email_reply.txt", "w", encoding="utf-8") as file:
file.write(reply)

25CS2506 – BUILDING APPLICATIONS USING GPT-4

print("\nEmail saved as email_reply.txt")

Sample Input / Expected Output
Received email: Please attend the department meeting tomorrow.
Tone: Professional
Output: The model generates a suitable subject and a polite confirmation reply, then saves it in
email_reply.txt.
Result
The program is executed successfully and the required output is obtained.

Experiment 4 – Personalized Recommendation System
Aim
To recommend courses based on a student's interests and skill level using a predefined course
catalogue and Ollama.
Sample Question
An online learning platform wants to recommend courses to students. Develop a Python program that
accepts a student's name, interest and skill level. Read the available courses from a CSV file, and use
Ollama to recommend three suitable courses from that list with a short explanation for each
recommendation.
Sample Dataset
Course Category Level
Python Programming Programming Beginner
Machine Learning Artificial Intelligence Intermediate
Data Science Analytics Beginner
Cyber Security Security Intermediate
Computer Networks Networking Beginner
Deep Learning Artificial Intelligence Advanced

Save the table above as `courses.csv` in the same folder as the Python program.
Python Program
from ollama_helper import ask_ollama
import csv
courses = []
with open("courses.csv", "r", encoding="utf-8") as file:
reader = csv.DictReader(file)

25CS2506 – BUILDING APPLICATIONS USING GPT-4

for row in reader:
courses.append(row)
print("PERSONALIZED COURSE RECOMMENDATION SYSTEM")
name = input("Enter student name: ")
interest = input("Enter your interest: ")
skill = input("Enter skill level (Beginner/Intermediate/Advanced): ")
prompt = f"""
You are a course recommendation assistant.
Student name: {name}
Student interest: {interest}
Student skill level: {skill}
Available courses:
{courses}
Recommend three suitable courses only from the available list.
Give a short reason for each recommendation.
Do not invent courses.
"""
result = ask_ollama(prompt)
print("\nRECOMMENDED COURSES\n")
print(result)

Sample Input / Expected Output
Name: Arun
Interest: Artificial Intelligence
Skill level: Beginner
Output: The model recommends suitable courses from the CSV catalogue and explains why they match
the student's interest and level.
Result
The program is executed successfully and the required output is obtained.

25CS2506 – BUILDING APPLICATIONS USING GPT-4
Experiment 5 – AI Document Analyzer: Summarize, Query and Export
Aim
To read a text document, generate a summary, answer a question based on the document and export
the results.
Sample Question
A college wants a tool to help faculty members analyse text documents. Develop a Python program
that reads a text file, displays its contents, generates a summary using Ollama, accepts a question
about the document, answers using the document as context, and exports the summary and answer to
a text file.
Sample Dataset
Create a file named ai.txt with the following content:
Artificial Intelligence is a branch of computer science. It enables
machines to perform tasks that normally require human intelligence.
Applications of AI include healthcare, education, transportation and
robotics. Machine learning is a subfield of AI that enables systems to
learn patterns from data.

Python Program
from ollama_helper import ask_ollama
print("AI DOCUMENT ANALYZER")
filename = input("Enter text file name (example: ai.txt): ")
try:
with open(filename, "r", encoding="utf-8") as file:
document = file.read()
print("\nDOCUMENT CONTENT:\n")
print(document)
summary_prompt = f"""
Summarize the following document in simple language.
Use only the information in the document.
Document:
{document}
"""
summary = ask_ollama(summary_prompt)
print("\nDOCUMENT SUMMARY:\n")
print(summary)

25CS2506 – BUILDING APPLICATIONS USING GPT-4

question = input("\nEnter your question about the document: ")
question_prompt = f"""
Answer the question using only the document below.
If the answer is not present, say:
Information not available in the document.
Document:
{document}
Question: {question}
"""
answer = ask_ollama(question_prompt)
print("\nANSWER:\n")
print(answer)
with open("analysis_result.txt", "w", encoding="utf-8") as output:
output.write("SUMMARY:\n" + summary)
output.write("\n\nQUESTION:\n" + question)
output.write("\n\nANSWER:\n" + answer)
print("\nResults saved as analysis_result.txt")
except FileNotFoundError:
print("File not found. Check the filename and try again.")

Sample Input / Expected Output
Use ai.txt as the input document.
Question: What are the applications of AI?
Expected answer: Healthcare, education, transportation and robotics.
Result
The program is executed successfully and the required output is obtained.

25CS2506 – BUILDING APPLICATIONS USING GPT-4

Experiment 6 – AI-Powered Medical Assistant
Aim
To develop a simple health information assistant that answers general questions using a predefined
information file and Ollama.
Sample Question
A health information centre wants a basic assistant to answer general health questions from a small
information file. Develop a Python program that reads the health information file, accepts a user's
question, sends the information and question to Ollama, and displays the response with a medical
disclaimer. The assistant must not diagnose diseases or prescribe medicines.
Sample Dataset
Create a file named health_info.txt with the following content:
Drinking sufficient water helps maintain hydration. Regular exercise
supports physical fitness. Fruits and vegetables provide essential
nutrients. Adequate sleep supports general wellbeing. Personal hygiene
helps prevent infections.

Python Program
from ollama_helper import ask_ollama
print("AI-POWERED MEDICAL INFORMATION ASSISTANT")
with open("health_info.txt", "r", encoding="utf-8") as file:
health_information = file.read()
print("\nAsk a general health information question.")
print("Type exit to stop.")
while True:
question = input("\nEnter your question: ")
if question.lower() == "exit":
break
prompt = f"""
You are a general health information assistant.
Use the information below to answer the question.
Do not diagnose diseases or prescribe medicines.
If the information is not available, say so.
Health information:
{health_information}
Question: {question}

25CS2506 – BUILDING APPLICATIONS USING GPT-4

"""
answer = ask_ollama(prompt)
print("\nAssistant:", answer)
print("\nDisclaimer: This is general educational information "
"and is not a substitute for professional medical advice.")

Sample Input / Expected Output
Question: Why is drinking water important?
Expected response: Drinking sufficient water helps maintain hydration. The application also displays
the medical disclaimer.
Result
The program is executed successfully and the required output is obtained.

25CS2506 – BUILDING APPLICATIONS USING GPT-4

Quick Reference: Files to Prepare
File name Used in Purpose
ollama_helper.py All six experiments Connects Python programs to
the local Ollama model.
products.csv Experiment 1 Contains product names, prices

and descriptions.
courses.csv Experiment 4 Contains course names,
categories and skill levels.

ai.txt Experiment 5

Sample document for
summarization and question
answering.

health_info.txt Experiment 6 General health information

used as context.

Troubleshooting
 Connection refused: Check that Ollama is installed and running.
 Model not found: Run `ollama list` and update the model name in ollama_helper.py.
 ModuleNotFoundError for requests: Run `pip install requests`.
 CSV or text file not found: Keep the dataset file in the same working folder as the Python program,
or provide its correct path.
 Slow response: Use a smaller model suitable for the computer's available RAM and allow sufficient
time for generation.

Note for Students
These programs are introductory reference implementations. The language model may produce
different wording for the same input. Always check that the answer is relevant to the supplied
information.
