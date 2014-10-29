Word-game-scrabble
==================
def getWordScore(word, n):

    
    
    letterscore = sum([SCRABBLE_LETTER_VALUES[c] for c in word])*len(word)
    
    #print letterscore
    
    if len(word) == n:
        letterscore += 50
        return letterscore
    else:
        return letterscore
        
        
def updateHand(hand, word):
    handcopy = hand.copy()
    for char in word:
        handcopy[char] = handcopy.get(char, 0) - 1
 
    return handcopy
    
def isValidWord(word, hand, wordList):
     
    handcopy = hand.copy()
    letterstring = []
    for key, value in handcopy.iteritems():
        letterstring.append(key*value)
        #print letterstring
        #print letterstring
        temp = []
        for char in letterstring:
            temp += char
            #print temp
        #print temp
        #eg. for wordinhand("rapture",{'r': 1, 'a': 3, 'p': 2, 'e': 1, 't': 1, 'u':1})
        #returns str "aaaepprut"
    
    
    #print "its here: " + str(temp)   
    
    condition = True
    for char in word:
        #print char
        if char not in temp:
            condition = False
        else:
            temp.remove(char)
                
    
    #print "its here: " + str(condition)
                            
    if word not in wordList:
        return False
 
    elif condition==True:
        return True
    else:
        return False
        
def calculateHandlen(hand):

    """ 
    Returns the length (number of letters) in the current hand.
    hand: dictionary (string int)
    returns: integer
    """
    # TO DO... <-- Remove this comment when you code this function
    
    if len(hand) < 1:
        return 0
    else:
        letterstring = []
        handcopy = hand.copy()
        for key, value in handcopy.iteritems():
            letterstring.append(key*value)
            #print letterstring
            #print letterstring
            temp = ""
            for char in letterstring:
                temp += char
                #print temp
            #print temp
            #. for wordinhand("rapture",{'r': 1, 'a': 3, 'p': 2, 'e': 1, 't': 1, 'u':1})
            #returns str "[a,a,a,e,p,p,r,u,t]"
    return len(temp)
    
def playHand(hand, wordList, n):
    totalscore = 0
    
    # As long as there are still letters left in the hand:
    
    while calculateHandlen(hand) > 0:
        # Display the hand
        print "Current Hand: " + str(hand)
        # Ask user for input
        userword = raw_input("""Enter word, or a "." to indicate that you are finished: """)
        # If the input is a single period:
        if userword == '.':        
            # End the game (break out of the loop)
            break
        # Otherwise (the input is not a single period):
        else:
            # If the word is not valid:
            if isValidWord(userword, hand, wordList) == False:
                        
                # Reject invalid word (print a message followed by a blank line)
                print "Invalid word, please try again.\n"
            # Otherwise (the word is valid):
            else:
                totalscore += getWordScore(userword, n)
                # Tell the user how many points the word earned, and the updated total score, in one line followed by a blank line
                print '"' + str(userword) + '" ' + "earned " +  str(getWordScore(userword, n)) + " points. Total: " + str(totalscore) + "points\n"
                #print str(userword) +  str(getWordScore(userword, n)) + str(totalscore)
                # Update the hand
                hand = updateHand(hand, userword)
    # Game is over (user entered a '.' or ran out of letters), so tell user the total score
    print "Run out of letters. Total score: " + str(totalscore) + "points." 
    
def playGame(wordList):
    """
    Allow the user to play an arbitrary number of hands.
 
    1) Asks the user to input 'n' or 'r' or 'e'.
      * If the user inputs 'n', let the user play a new (random) hand.
      * If the user inputs 'r', let the user play the last hand again.
      * If the user inputs 'e', exit the game.
      * If the user inputs anything else, tell them their input was invalid.
 
    2) When done playing the hand, repeat from step 1
    """
    var = 10
    hand = {}
    while var > 0:
        var = var - 1
        choice = raw_input("Enter n to deal a new hand, r to replay the last hand, or e to end game: ")
        
        
    
        #if choice == r or choice ==n
    
    
        if choice == 'n':
            #print hand
            hand=dealHand(HAND_SIZE)
            #print hand
            playHand(hand, wordList, HAND_SIZE)
            #print  "hand after choice 'n' " + str(hand)  
            continue
         
            
        elif choice == 'r':
            if hand == {}:
                #print "hand after choice 'r', empty {}" + str(hand) 
                print "You have not played a hand yet. Please play a new hand first!"
            else:
                #print hand
                playHand(hand, wordList, HAND_SIZE)
            continue
            
        elif choice == 'e':
            break
        else:
            print "Invalid command."
            return playGame(wordList)
            
def compChooseWord(hand, wordList, n):

    """
    Given a hand and a wordList, find the word that gives 
    the maximum value score, and return it.
    This word should be calculated by considering all the words
    in the wordList.
    If no words in the wordList can be made from the hand, return None.
    hand: dictionary (string -> int)
    wordList: list (string)
    returns: string or None
    """
    
    highestscore = 0
    # Create a new variable to store the best word seen so far (initially None) 
    bestword = "" 
    # For each word in the wordList
    for word in wordList:
        # If you can construct the word from your hand
        if isValidWord(word, hand, wordList) ==True:
        # (hint: you can use isValidWord, or - since you don't really need to test if the word is in the wordList - you can make a similar function that omits that test)
            
            # Find out how much making that word is worth
            score = getWordScore(word, n)
            # If the score for that word is higher than your best score
            if score > highestscore:
                highestscore = score
                bestword = word
                # Update your best score, and best word accordingly
    if bestword == "":
        return None
    else:
        return bestword
        
def compChooseWord(hand, wordList, n):

    """
    Given a hand and a wordList, find the word that gives 
    the maximum value score, and return it.
    This word should be calculated by considering all the words
    in the wordList.
    If no words in the wordList can be made from the hand, return None.
    hand: dictionary (string -> int)
    wordList: list (string)
    returns: string or None
    """
    
    highestscore = 0

    # Create a new variable to store the best word seen so far (initially None) 
    
    bestword = "" 

    # For each word in the wordList
    
    for word in wordList:
        # If you can construct the word from your hand
        if isValidWord(word, hand, wordList) ==True:
        # (hint: you can use isValidWord, or - since you don't really need to test if the word is in the wordList - you can make a similar function that omits that test)
            
            # Find out how much making that word is worth
            score = getWordScore(word, n)
            # If the score for that word is higher than your best score
            if score > highestscore:
                highestscore = score
                bestword = word
                
                # Update your best score, and best word accordingly
            
    if bestword == "":
        return None
    else:
        return bestword

def compPlayHand(hand, wordList, n):

    """
    Allows the computer to play the given hand, following the same procedure
    as playHand, except instead of the user choosing a word, the computer 
    chooses it.
    1) The hand is displayed.
    2) The computer chooses a word.
    3) After every valid word: the word and the score for that word is 
    displayed, the remaining letters in the hand are displayed, and the 
    computer chooses another word.
    4)  The sum of the word scores is displayed when the hand finishes.
    5)  The hand finishes when the computer has exhausted its possible
    choices (i.e. compChooseWord returns None).
    hand: dictionary (string -> int)
    wordList: list (string)
    n: integer (HAND_SIZE; i.e., hand size required for additional points)
    """
    
    # TO DO ...
    totalscore = 0
    
    while calculateHandlen(hand) > 0:
        print "Current Hand: " +str(hand)
        compword = compChooseWord(hand, wordList, n)
        
        if compword == None:
            break
        else:
            totalscore += getWordScore(compword, n)
            print '"' + str(compword) + '" ' + "earned " +  str(getWordScore(compword, n)) + " points. Total: " + str(totalscore) + "points\n"
            hand = updateHand(hand, compword)
    print "Total score: " +str(totalscore)+ " points."
    
