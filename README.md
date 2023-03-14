# Reversing-apk-with-smali
In this guide, we will go through the steps of doing Reversing with Smali.

We need 3 tools: \
a. Apktools 2.7.0 \
b. keytool.jar \
c. zipalign (as part of android 4.4.2 decode) 

Steps: 

1. Get the main package name (you can find in JADX or in sandbox)
hint: You can go to the "Manifist" and search for "MainActivity" withput any number. this is will be the landing package. The landing activity will be in <intent-filter> method.
2. We have to search for a boolean function, that her goal is to detect emulator model. one of the way to do it, is to search for a function name "build model". this is a string method that return the name of the model in your device. 
3. On Jadx click "Smali"
4. Search for the specific line as the function that detect the emulator showing. for example, if the line is 72, we will search for "line 72" in the smali method.
5. We should look at the return statemant. It will looks like this (0X1) meaning an binary choise between True and False. we want to change it to 0X0, so it will return false boolean, no matter the condition. 
6. We will decompile the file, first open cmd:

> java -jar "***path of apktools***" d "***apk to decompile***" -o "***namef new folder***"

 You will find the folder in the path showing in the cmd. 

 7. We will change the smali code, inside the main folder pacage. (you can look at the original java, there is a line named "changed from x" - this is the function we want to find in the smali folder. 

 8. now we will compile the dex to apk again:

   >  java -jar "***path of apktools***" b "***path the new folder***" -o "***new name of apk***".apk

9. we will male zipalign to ensure that all of the classes are compiled:
> "***zipalign path***" -v 4 "***path of app***" "***new name of app***" 

10. We will create a key for sumbit our app:  

 > "***keytool path***" -genkey -v -keystore "***name of key***".jks -keyalg RSA -keysize 2048 -validity 10000 -alias myAlias \
11. at the end, we will complie the app together with the key and the zipalign apk:
> "***apksigner app path***" sign —ks "***path key***".jks —out "***path of new apk***".apk "***new sigened name of apk***".apk
