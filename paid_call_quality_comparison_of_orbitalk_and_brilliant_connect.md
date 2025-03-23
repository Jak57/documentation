## Paid Call Quality Comparison of Orbitalk and Brilliant Connect

## Goal
Compare the voice quality of Orbitalk and Brilliant Connect during the app to the GSM (Global System for Mobile Communication) call.

## Experimental Setup
For experimenting with voice quality measurement two Android devices are needed. One device is the one from where a call will be made and that device must have Orbitalk and Brilliant Connect installed. The calling device will remain constant but the receiving device will vary. From the calling device, using Orbitalk or the Brilliant Connect app GSM call will be made to the receiving device. During the call, the participant who initiated the call will only speak using the Orbitalk or Brilliant Connect app. Voice will be processed by the app the will be sent to the receiving participant who is a GSM user.  

The receiving participant will observe the call and provide a rating from 1 - 5 (where 1 means bad quality and 5 means excellent quality). From the ratings of different calls Mean Opinion Score will be calculated.

## Template for Collecting Ratings from Participants
For collecting ratings of calls from different participants a template is developed which will explain the experimental scenario to them. The template is provided below:

```
**Paid Call Test:**

Hello, I hope you are well. 

I am conducting some tests to compare the voice quality of paid audio calls. For that purpose, I have to make 4 paid calls to different people and collect their opinions on the quality of the audio call. 



** During the call, I will read random newspapers.

** The other participant does not need to say anything during the call, he should observe the quality of the call.

** At the end of the call he has to give a rating of the quality of the call on a scale of 1-5 (where 1 means bad and 5 means excellent.) The rating can be any fractional number between 1 and 5. 

** Please note down the rating in the following way:
Call1 -> #rating 
Call2 -> #rating

** From the rating, the MOS score (Mean Opinion Score) will be calculated to assess the quality of the audio calls.

** If you are free now to receive the call, please let me know and provide your phone number please.

** When the 2 call ends please provide the ratings in this format.
Call1 -> #rating 
Call2 -> #rating
```

## Audio Processing Pipeline in Orbitalk
In the Orbitalk app the following processing is carried out after capturing audio using a microphone in the following order:
1. Webrtc APM
2. Dynamic Range Compression (DRC)
3. RNNoise Canceller
4. Comfort Noise Generation (CNG)

## Experimental Results
The experimental results are provided in the following table:

| Participant | Call Number | Orbitalk (Rating) | Brilliant Connect (Rating) |
|:-----------:|:-----------:|:-----------------:|:--------------------------:|
| Fuji | 1 | 3.5 | 2.5 | 
| Fuji | 2 | 3.5 | 3.75 | 
| Fuji | 3 | 2.5 | 3.5 | 
| Mahin | 1 | 4 | 3.5 | 
| Mahin | 2 | 4.5 | 4.8 | 
| Shoriful | 1 | 3 | 3.5 | 
| Shoriful | 2 | 3 | 3 | 
| Atik | 1 | 4 | 3.5 | 
| Atik | 2 | 4 | 4 | 
| Fuji | 1 | 3.5 | 2.5 | 
| Fuji | 2 | 3.5 | 2.5 | 
| Fuji | 1 | 3.5 | 2.5 | 
| Fuji | 2 | 3.5 | 2.5 | 






