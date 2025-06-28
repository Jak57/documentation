## Basics of Acoustic Echo

In ``audio/video conferencing``, when any participant speaks, his/her voice is recorded through the ``microphone``. At that time, the participant's ``speaker`` plays the received voice from the network. If the participant uses a speaker with ``high volume``, the delayed version of the received voice is picked up by the microphone along with the participant's voice. This delay is introduced due to the ``internal delay`` of the microphone, speaker, and OS/hardware. "received voice" from other participants being played at speaker is called the ``far-end`` signal, and the microphone signal along with the delayed received voice is called the ``near-end`` signal.

Due to the addition of the received voice with the participant's audio, an ``echo`` is generated and can be experienced by other participants if the participant whose device causes the echo, cannot cancel that.

When ``no echo`` is generated:

``Far-end = received audio``<br>
``Near-end = microphone audio``


When ``echo`` is generated:

``Far-end = received voice``<br>
``Near-end = microphone audio + (shifted/delayed) received voice``


So, the goal of the ``echo canceller`` is to remove the received voice (far-end signal) being played at speaker from the participant's own audio (near-end signal) in microphone to keep only the participant's voice.

```
EchoCanceller(Near-end, Far-end):  
=> Near-end - Far-end
=> (Microphone signal + delayed/shifted Speaker signal) - (speaker signal)
=> (Participant's voice + delayed/shifted Received voice) - (Received voice)
=> (echo canceled) Participant voice 
```

## Delay: Simplified Range
* For syncing near-end and far-end audio signals, the WebRTC AEC performs well for an initial delay value of 240ms for a delay range of 60ms to 190ms. 

* For delay range from 200ms to 310ms, the initial delay value of 490ms works well.

## Order of ``cancelEchoDataShort()`` and ``feedPlayedData()`` of WebRTC AEC

* ``cancelEchoDataShort()`` must be placed before ``feedPlayedData()``.

* If we process 60ms of audio data in both ``cancelEchoDataShort()`` and ``feedPlayedData()`` the performance is lower than the 10ms processing.

* 10ms in ``cancelEchoDataShort()`` and 60ms processing in feedPlayedData() improves the quality.

* The highest quality of echo canceled audio is achieved with the 10ms processing.

## Delay: GCC-PHAT coverage
* With near-end and far-end audio of duration ``1s 260millis``, the ``GCC-PHAT`` algorithm can correctly measure delay up to ``310ms``.

---
# Simulation of evaluating the robustness of WebRTC AEC (Acoustic Echo Canceller)

## Delay: Changing delay value 

A delay of 250ms is introduced at the beginning of the far-end audio signal. The audio is merged with the near-end audio signal to generate the audio signal with echo. This signal is given to the ``cancelEchoDataShort()`` of WebRTC AEC with a chunk size of 10ms. After that, the far-end signal is provided to the ``feedPlayedData()`` with a chunk size of 60ms. The near-end audio is processed with a chunk size of 10ms and the far-end audio is processed with a chunk size of 60ms.

Instead of initializing the WebRTC AEC with a 250ms delay, we experimented with a varying range of delay from 0ms to 500ms. The performance of the echo canceller after initialization with varying delays is provided below:

* When decreasing the delay value from ``250ms`` to downward until ``0ms`` the performance of the echo canceller degraded. But the quality of the resulting audio is not bad.

* Increasing the delay value starting from ``250ms`` up until ``490ms``, the performance improved. The performance of the echo canceller for the delay value of 490ms is much better than the original delay value of 250ms.

* In all the experiments, echo is completely removed after around ``15s``.


## Delay: Introducing delay in the beginning and middle

In this experiment, the primary goal was to evaluate the performance of the echo canceller for desync far-end audio signal. For this purpose, the far-end audio is delayed with varying amounts of delay value starting from 10ms to 500ms. Then, an initial delay of 250ms is introduced at the beginning of the modified far-end audio. After that, the mic audio is combined with the delayed audio to generate the audio with echo. The WebRTC AEC processed the near-end audio first and then the far-end audio.

The findings of this experiment are provided below:
* When the value of the delay introduce after ``5000ms`` increased gradually from ``10ms`` to ``500ms`` the performance of the echo canceller degraded significantly.

* Until the ``350ms`` delay, the echo canceller can ``cancel`` the echo within ``15s``. After 15s, no echo is present.

* For a ``450ms`` delay, the echo is canceled within 25ms. But, for ``higher delay`` values the echo canceller ``can not cancel`` the echo.


## Delay: Changing delay in run-time

In this experiment, the primary goal was to evaluate the performance of the echo canceller for desync far-end audio signal with updated delay value at run-time. For this purpose, the far-end audio is delayed with varying amounts of delay value starting from 10ms to 500ms. Then, an initial delay of 250ms is introduced at the beginning of the modified far-end audio. After that, the mic audio is combined with the delayed audio to generate the audio with echo. The WebRTC AEC processed the near-end audio first and then the far-end audio.

The findings of this experiment are provided below:

* The range of delay initialization in WebRTC AEC ranges from 20ms to 500ms. So, as we introduced an initial delay of ``250ms``, we can update the delay at run-time by at most ``250ms`` (250 + 250 = 500).

* Previous experiments with changing delay values reveal that for an initial delay value of 250ms, the values upper than 250ms to 490ms work properly.

* Updating the delay value after ``5000ms`` with the initial delay of ``250ms plus the added delay`` in the middle, ``no noticeable improvement`` in the echo canceled audio is observed for middle delay values from 10ms to 240ms due to the observation stated above.

## Delay: Changing delay in run-time due to delay introduced by mic and speaker

In this experiment, the far-end signal is delayed by 160ms. The delayed signal is then added to the mic signal. The mixed signal is delayed by 60ms/120ms/180ms (multiples of 60ms: duration of one audio frame). This delay is introduced due to the internal delay of the mic and speaker. For the cancellation of the echo, the near-end signal is processed with a chunk size of 10ms, and the far-end signal is processed with a chunk size of 60ms.
 
The findings of this experiment are provided below:

* After 5000ms if we introduce a delay of ``60ms``, no effect is visible in the output audio signal. Echo is ``fully canceled`` in this case when initializing the WebRTC echo canceller with a delay of 160ms.

* Introduction of delay more than 60ms such as 120ms/180ms causes problem. The WebRTC echo canceller can not cancel echo with the default initialization value of 160ms.

* ``Updating the initial delay value`` (160ms) with the addition of 120ms/180ms (160 + 120, 160 + 180) resulted in ``better echo canceled audio`` signal.  


## Changing delay in run-time due to skipping of microphone recording

In this experiment, the reference audio signal is delayed by ``160ms`` from the beginning and the delayed signal is added with the microphone signal. In the mixed signal, after 5000ms a skip of 60ms/120ms/180ms is introduced and the later part of the mixed signal is shifted left. The shifted signal acts as the near-end signal.

The findings from this experiment are provided below:

* Increasing the amount of skip duration ``(60 -> 120 -> 180ms)`` resulted in poorer performance.

* The performance ``without the subtraction is much better``.

* This experiment, indicates that ``setting a larger delay`` value doesn't hurt and can be ``beneficial``.

## Delay: Effect of delay introduced by speaker

In this experiment, the reference signal is delayed by ``160ms`` at the ``beginning``. After ``5020millis`` a delay of ``10020ms`` is introduced and the resulting audio is mixed with the microphone audio. This audio acts as the near-end audio. 

The near-end audio is processed with a chunk size of 10ms and the far-end audio signal is processed with a chunk size of 60ms. After 5020ms the far-end audio is not processed for the next 10020ms whereas the near-end audio is processed after every 10ms.

The findings of this experiment are provided below:

* ``Increasing the delay`` after 5020ms due to the speaker, ``no effect`` is introduced in the echo canceled audio. Because, after 5020ms, for the next 10020ms only the microphone's data is recorded which is not corrupted by the echo. So, in that portion, the echo canceller does not need to cancel any echo. After the portion of 10020ms, the mic is ``still aligned`` with the far-end signal with a delay of 160ms. So, when the speaker starts working again after the delay of 10020ms it is still aligned with the near-end signal with the delay of 160ms. So, the delay value does not need any change.

## Best performance
We have achieved the optimal echo cancellation performance by utilizing delays caused by the speaker and microphone in real-time and updating the WebRTC delay value with this delay measurement.


---
**Prepared by**<br>
*Jakir Hasan (Reve Systems'24)*<br>
*Date (creation) - 04/10/24*<br>
*Date (last modification) - 04/11/24*<br>


**Supervised by**<br>
*Md. Maniruzzaman Monir*<br>
*Nafiul Alam Fuji*<br>
