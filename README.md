# Virtual-Assistant-Horizon-
#This is the very basic code at the moment will add more queries and responses ; But first step is to put everything in functions and classes
from gtts import gTTS
import os
from selenium import webdriver
from selenium.webdriver.support.select import Select
import speech_recognition as sr
import time
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from keyboard import press
r = sr.Recognizer()
language = 'en'
l1=['what is the time','tell me the time','what is the current time']
l2=['horizon','hay horizon','hello horizon','hey horizon','hello Horizon','hay Horizon','Horizon','hello Rising','hay Rising','Rising','hello rising','hay rising','rising','hello ryzen','hay ryzen','hello Ryzen','hay Ryzen','Hey ho Rising','hey ho rising','Hey ho rising','hello risin','HIV reason','HIV risin','hello listen','ryzen']
l3=['thanks','Thanks','thank you','Thank You','Thank you','thank You',]
l4=['play','play me a song','Play me a song','Play','play Me A Song']
l5=['yes','Yes']
l6=['no','No']



#initail sentence stratup
os.system('setup.mp3')
time.sleep(13)

#continue setup name asking
fin=open("name.txt",'w')
name = input('enter name')
fin.write(name)
fin.close()
mytext = "hello,"+name+",to ask me for help start by saying hello horizon, or hey horizon or simply horizon "
mymp3 = gTTS(text=mytext, lang=language, slow=False)
mymp3.save('name.mp3')
os.system('name.mp3')
time.sleep(13)

mytext = "hello,"+name+",how can I help you today "
mymp3 = gTTS(text=mytext, lang=language, slow=False)
mymp3.save('welcome.mp3')



run = True
activated = False


while run == True:
    if activated==False:
        with sr.Microphone() as source:
            audiotxt = r.listen(source)
        try:
            speech = (r.recognize_google(audiotxt))
            print(speech)



            if speech in l2:
            
                os.system('intro_sound.mp3')
                time.sleep(1)
                
                os.system('welcome.mp3')
                time.sleep(4)
                activated = True
        except:
            pass
        




    if activated==True:
        print('talk')
        print('-----------------------------------------------------------------------------------------------------------------------------------------------------')
        with sr.Microphone() as source:
            audiotxt = r.listen(source)
        try:
            speech = (r.recognize_google(audiotxt))
            print(speech)

            if speech in l1 :
                os.system('intro_sound.mp3')
                t = time.localtime()
                current_time = time.strftime("%H:%M:%S", t)
                mytxt = 'the current time in the United Arab Emirates is'+current_time
                mymp3 = gTTS(text=mytxt, lang=language, slow=False)
                mymp3.save('time.mp3')
                os.system('time.mp3')
                time.sleep(8)
                os.remove('time.mp3')
                activated = False

            
            if speech in l4:

                mytxt = 'okay which song do you want to hear'
                mymp3 = gTTS(text=mytxt, lang=language, slow=False)
                mymp3.save('song.mp3')
                os.system('song.mp3')
                song=input('enter the song you want to hear')
                time.sleep(3)
                browser = webdriver.Chrome("chromedriver")
                browser.get("https://www.youtube.com/?gl=AE&tab=r1")
  
                search = browser.find_element_by_name('search_query')
                search.send_keys(song)
                search_button = browser.find_element_by_id("search-icon-legacy")
                search_button.click()
                time.sleep(25)
                
                links=[]
                links.append(browser.find_element_by_id('video-title').get_attribute('href'))
                vid = links[0]
                print(vid)
                driver = webdriver.Chrome("chromedriver")
                driver.get(vid)
                play=driver.find_element_by_id('player-container')
                play.click()
                
                browser.close()
                activated = False



            if speech not in l1 or l2 or l3 or l4:
                mytxt = 'I am not sure how to respond to that, but I can search the web for it, do you want me to do that'
                mymp3 = gTTS(text=mytxt, lang=language, slow=False)
                mymp3.save('web_search.mp3')
                os.system('web_search.mp3')
                time.sleep(8)
                print('talk')
                print('----------------------------------------------------------------------------------------------------------------------------------------')
                with sr.Microphone() as source:
                    audiotxt = r.listen(source)
                answer = (r.recognize_google(audiotxt))
                print(answer)
                if answer in l5:
                    browser = webdriver.Chrome("chromedriver")
                    browser.get("https://www.google.com/")
                    search_bar=browser.find_element_by_name('q')
                    search_bar.send_keys(speech)
                    press('enter')
                    time.sleep(2)
                    speak = browser.find_element_by_class_name('Z0LcW XcVN5d AZCkJd')
                    final_speech = speak.get_attribute('data-tts-text')
                    mymp3 = gTTS(text=mytxt, lang=language, slow=False)
                    mymp3.save('random_web_search.mp3')
                    os.system('random_web_search.mp3')
                    
                    activated = False
                elif answer in l6:
                    activated = False







        
            
            if speech in l3 :
                os.system('thanks.mp3')
                time.sleep(4)
                activated = False
    

            
        
        except:
            os.system('repeat.mp3')
            time.sleep(4)
            activated = False
        
