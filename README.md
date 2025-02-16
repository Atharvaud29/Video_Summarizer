## Youtube Video Transcribe Summarizer LLM App with the help of Google Gemini Pro(API_key)

#Project Goal :

To create an LLM app which can summarizer the given video (any video in English) under 300 words for quick understanding. For this you have to provide the youtube video URL or link to the summarizer. The model will fetch data from the video, process it and provide the right summary to understand the information without watching the video.

#Working :

1. Create an Google Gemini Pro API key and then paste the key value in .env file

       GOOGLE_API_KEY = "Your key value"

2. Now install all the libraries in requirements.txt file

       youtube_transcript_api
       streamlit
       google-generativeai
       python-dotenv
       pathlib

3. Then write the code in app.py

  a. import all the required package's

    import streamlit as st
    from dotenv import load_dotenv
    load_dotenv() ##load all the nevironment variables
    import os
    import google.generativeai as genai
    from youtube_transcript_api import YouTubeTranscriptApi
    
  b. store your API key in genai.configure

    genai.configure(api_key=os.getenv("GOOGLE_API_KEY"))

  c. Provide the Prompt to your summarizer. The summarizer will do exactly the same as prompt say

    prompt="""You are Yotube video summarizer. You will be taking the transcript text
    and summarizing the entire video and providing the important summary in points
    within 300 words. Please provide the summary of the text given here:  """

  d. Now get the data from Youtube 

    def extract_transcript_details(youtube_video_url):
        try:
           video_id = youtube_video_url.split("=")[1]
           transcript_text = YouTubeTranscriptApi.get_transcript(video_id)
           transcript = ""
           for i in transcript_text:
               transcript += " " + i["text"]
           return transcript 
         except Exception as e:
             raise e

  e. Then get summary based on prompt

    def generate_gemini_content(transcript_text,prompt):
        model=genai.GenerativeModel("gemini-pro")
        response=model.generate_content(prompt+transcript_text)
        return response.text

  f. Now create Streamlit application

    st.title("YouTube Transcript to Detailed Notes Converter")
    youtube_link = st.text_input("Enter YouTube Video Link:")
    if youtube_link:
        video_id = youtube_link.split("=")[1]
        print(video_id)
        st.image(f"http://img.youtube.com/vi/{video_id}/0.jpg", use_container_width=True)

    if st.button("Get Detailed Notes"):
        transcript_text = extract_transcript_details(youtube_link)

        if transcript_text:
            summary = generate_gemini_content(transcript_text, prompt)
            st.markdown("## Detailed Notes:")
            st.write(summary)

4. To run this use the following command in terminal
  
       streamlit run app.py

#Output :

![Screenshot 2025-02-16 221411](https://github.com/user-attachments/assets/33b91ee4-6145-4ff0-8c90-43187eaaf8f8)

After you insert the URL
Check of Get Detailed Notes, Your summary will generate

![Screenshot 2025-02-16 222334](https://github.com/user-attachments/assets/8206b31c-a21f-4dc3-afec-25f72ba45267)


So this is how you can create your own customise Video Summarizer LLM App using Google Gemini API. 




    
