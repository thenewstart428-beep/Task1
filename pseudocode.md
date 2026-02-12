FUNCTION new_customer_form()
    BEGIN FUNCTION
    SET name TO ""
    SET surename TO""
    SET dob TO ""
    ## name
    WHILE name TO "" Do
        SEND "Please enter your name" TO DISPLAY
        RECIEVE name FROM(STRING) KEYBOARD
    ENDWHILE
    ## surname
    WHILE surename TO "" Do
        SEND "Please enter your name" TO DISPLAY
        RECIEVE surename FROM(STRING) KEYBOARD
    ENDWHILE
    ##
    SET today TO todaydate()
    WHILE dob TO "" DO
        SEND "Please enter your date of birth" TO DISPLAY
        RECIEVE dob FROM(DATETYPE) KEYBOARD
        IF dob > today THEN
            SEND "The date of birth can not exceed today's date"
            RECIEVE dob FROM(DATETIME) KEYBOARD
        ENDIF
    ENDWHILE
    RETURN "Customer added"
ENDFUNCTION

## Username Checker
FUNCTION username_checker()
    BEGIN FUNCTION
    SET flag TO True
    WHILE flag = True DO
        SET username TO RECIEVE username FROM(STRING) KEYBOARD
        IF 3 < LEN(username) < 13 THEN
            RETURN "Username accepted " TO DISPLAY
            SET flag TO False
            ENDWHILE
        ELSE:
            SEND "The length of username should be between 4-12 characters" TO DISPLAY
        ENDIF
ENDFUNCTION

## Stock Level Alert
FUNCTION stock-level()
    BEGIN FUNCTION
    SET stock_level TO RECIEVE stocks FROM(FLOAT)KEYBOARD
    IF stock_level = 0 THEN 
        SEND "Out of stocks" TO DISPLAY
    ELSE IF 0 < stock_level < 6 THEN
        SEND "Low stock" TO DISPLAY
    ELSE IF 5< stock_level <21 THEN
        SEND "Stock OK"
    ELSE IF stock level > 20 THEN
        SEND "High stock"
    ELSE THEN
        SEND "Sorry the input was not valid please try again later"
    ENDIF
ENDFUNCTION


##  Maintenance Due Checker

FUNCTION maintenance()
    BEGIN FUNCTION
    SET daysSinceLastService TO RECIEVE days FROM(INTEGER)KEYBOARD
 
    WHILE daysSinceLastService NOT number DO
        SEND "Please enter a number"
        SET daysSinceLastService TO RECIEVE days FROM(INTEGER)KEYBOARD
    ENDWHILE
 
    WHILE daysSinceLastService < 0 DO
        SEND "The number must be 0 or more!"
        SET daysSinceLastService TO RECIEVE days FROM(INTEGER)KEYBOARD
    ENDWHILE
 
    SET serviceFrequency TO ""
    SEND "Please choose an option for you service frequency: 1.Weekly  2.Monthly" TO DISPLAY
    SET frequency TO RECIEVE frequency FROM(INTEGER) FROM KEYBOARD
 
    IF frequency = 1 THEN
        SET serviceFrequency TO 7
    ELSE IF frequency = 2 THEN
        SET serviceFrequency TO 30
    ELSE
        SEND "Sorry you did not enter a valid option " TO DISPLAY
    ENDIF
 
    SET daysleft TO serviceFrequency - daysSinceLastService
    IF daysleft IN [1,2] THEN
        SEND "Due soon" TO DISPLAY
    ELSE IF daysleft > 2 THEN
        SEND "Not due yet" TO DISPLAY
    ELSE IF daysleft = 0 THEN
        SEND "Due now" TO DISPLAY
    ELSE IF daysleft<1 THEN
        SEND "You are late for the service"
    ELSE
        SEND "There was a problem"
    ELIF
   
    RETURN "Service day function complete" ## You must return something always 
ENDFUNCTION

## Priority Classifier
FUNCTION priority_checker()
    BEGIN FUNCTION
    
    ## machine condition
    SET condition TO 0
    SEND "Please choose an option: 1.Good 2.Worn 3.Critical" TO DISPLAY
    condition = RECIEVE condition FROM (INTEGER)KEYBOARD
    WHILE condition NOT IN [1,2,3] DO
        SEND "Please choose a valid option"
        condition = RECIEVE condition FROM (INTEGER)KEYBOARD
    ENDWHILE

    ## last service day
    SET lastday TO 0
    SEND "Days since last service?" TO DISPLAY
    lastday = RECIEVE days FROM (INTEGER) KEYBOARD
    WHILE lastday<1 DO
        SEND "The minum number for this field is 0!"
        lastday = RECIEVE days FROM (INTEGER) KEYBOARD
    ENDWHILE

    ## Is the machine being used 
    SET inuse TO ""
    SEND "Is the machine currently in use (Yes/No) ?"
    inuse = RECIEVE use FROM(STRING) KEYBOARD.lower()
    WHILE inuse NOT IN ["yes","no"] DO 
        SEND "Please choose a valid option of yes or no!"
        inuse = RECIEVE use FROM(STRING) KEYBOARD.lower()

    IF condition = 3 OR lastday>59 THEN
        SEND "The task is in high priority list" TO DISPLAY
    ELSE IF condition = 2 OR 29 < lastday <60 THEN
        SEND "The task is in Medium priority list" TO DISPLAY
    ELSE IF condintion = 1 AND lastday<30 THEN
        SEND "The task is in Low priority list"
    ENDIF
    RETURN "Check has been done succefuly + "
ENDFUNCTION





 