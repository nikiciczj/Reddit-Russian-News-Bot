Reddit-Russian-News-Bot
=======================

Sends links to new /r/worldnews threads on Russian/Ukrainian conflict

import time
import praw

#Tell Reddit what bot does
user_agent = "Russia related thread monitor by /u/LoveOfProfit 0.1"

#Connect to Reddit
r = praw.Reddit(user_agent=user_agent)

# Login with credentials
r.login(username, password)

#Subreddit name
#sub = 'news','worldnews'

# Store submissions already found
already_done = []

#Desired keywords
prawWords = ['Russian','Russia','Ukraine']

#Title of message sent
#title = 'Russian thread'

#Get desired subreddit
subreddit = r.get_subreddit('worldnews')

while True:
	for submission in subreddit.get_new(limit=10):
		title = submission.title.format()
		has_praw = any(string in title for string in prawWords)
		#Check if keyword is in title and if found before
		if submission.id not in already_done and has_praw:	
			msg = '[Russia related thread](%s)' %submission.short_link
			
			#Notify in command
			print("Found one!")
			
			#Send message to desired user
			r.send_message(username, 'Russia Thread', msg)
			
			#Add submission to already_found
			already_done.append(submission.id)
	time.sleep(60)
