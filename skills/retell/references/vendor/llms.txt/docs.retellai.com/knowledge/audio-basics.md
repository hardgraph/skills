> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Audio Basics

> Audio fundamentals for building voice AI — sampling, quantization, codecs, sample rates, and how audio is represented digitally in telephony systems.

### How is Audio Represented Digitally

Sound waves are captured by a microphone, which converts the acoustic energy
into electrical analog signals. The analog signals are then fed into an ADC
(Analog-to-Digital Conversion). Here, two critical processes occur - sampling
and quantization.

<Card title="Sampling" color="#ca8b04">
  <Steps>
    <Step title="Definition">
      Sampling is the process of measuring the amplitude of an analog signal at
      regular intervals. These intervals are determined by the sample rate,
      expressed in Hertz (Hz). For example, a sample rate of 44.1 kHz means the
      signal is sampled 44,100 times per second.
    </Step>

    <Step title="Purpose">
      By sampling the audio signal, we create a series of discrete data points
      that approximate the continuous analog waveform.
    </Step>

    <Step title="Implication">
      The Nyquist Theorem states that the sample rate must be at least twice the
      highest frequency component in the audio signal to accurately reconstruct
      the original signal. For example, human hearing typically ranges up to 20
      kHz, hence the standard CD sample rate of 44.1 kHz.
    </Step>
  </Steps>
</Card>

<Card title="Quantization" color="#ca8b04">
  <Steps>
    <Step title="Definition">
      Quantization is the process of converting each sampled amplitude value
      into a digital value. This involves assigning a specific numerical value
      (quantization level) to each sample, based on its amplitude.
    </Step>

    <Step title="Purpose">
      The range of possible amplitude values is divided into discrete steps.
      Each step is assigned a digital value. The bit depth determines the number
      of possible quantization levels. For instance, a 16-bit system can
      represent 65,536 (2^16) different levels.
    </Step>

    <Step title="Implication">
      Quantization introduces a small amount of error, known as quantization
      noise, because the process involves rounding the true amplitude value to
      the nearest quantization level. Higher bit depths can reduce this error,
      leading to higher fidelity audio.
    </Step>
  </Steps>
</Card>

### Terminology

<Tabs>
  <Tab title="Sample Rate">
    The sample rate is the number of samples of audio carried per second. It's
    measured in Hertz (Hz).
  </Tab>

  <Tab title="Channel Count">
    This refers to the number of separate audio channels (e.g., mono, stereo,
    surround sound) in the recording. <br />
    Mono means single channel. All audio is combined into one channel.
  </Tab>

  <Tab title="Bit Depth">
    Bit depth refers to the number of bits used to represent each audio sample.
  </Tab>
</Tabs>

### Audio Encoding

Audio encoding refers to the process of converting audio data into a format that
can be easily stored, transmitted, and decoded by audio playback devices. This
process often involves compression to reduce file size while trying to maintain
the quality of the original audio. There are several popular audio encoding
formats, each with its own specific use cases and characteristics.

Here are some examples:

<Tabs>
  <Tab title="PCM">
    * `Description`: PCM (Pulse Code Modulation) is the most straightforward form of digital audio
      encoding. It represents the amplitude of the audio signal at uniformly spaced
      intervals.

    * `Usage`: It's the standard form of digital audio in computers, CDs, digital
      telephony, and other digital audio applications.
  </Tab>

  <Tab title="MP3">
    * `Description`: MP3 (MPEG Audio Layer III) is a lossy compression format that significantly reduces
      file size by removing audio data considered less important to human hearing.

    * `Usage`: It was widely used for music distribution and playback due to its
      ability to reduce file size while maintaining a decent level of audio quality.
  </Tab>

  <Tab title="AAC">
    * `Description`: AAC (Advanced Audio Coding) is a more advanced form of lossy compression than MP3,
      offering better audio quality at similar bitrates.

    * `Usage`: It’s commonly used in online streaming services, Apple's iTunes, and
      YouTube.
  </Tab>

  <Tab title="Opus">
    * `Description`: Opus is a versatile, open standard audio codec. It provides
      low latency and high-quality audio.

    * `Usage`: It’s widely used for real-time applications like video conferencing,
      VoIP, and streaming.
  </Tab>

  <Tab title="μ-law">
    * `Description`: μ-law (mu-law or ulaw) encoding is a non-linear audio encoding technique used
      in telephony. It compresses dynamic range, emphasizing quieter sounds for
      improved clarity.

    * `Usage`: Predominantly used in North American and Japanese telephone systems,
      it's integral to the G.711 telephony standard, enhancing voice transmission
      quality.
  </Tab>
</Tabs>

Note that audio encoding is not the same as audio format. An audio format refers
to the entire structure of the audio file, which includes the encoding, but also
encompasses other elements like metadata, file headers, and containers. For
example, a WAV file typically uses PCM encoding, and has its own header that
specifies audio sample rate, number of samples, etc.

### PCM Audio Representation

When audio is played, it is typically decoded into PCM (Pulse Code Modulation).
This process is true for most digital audio systems, regardless of the original
audio format or encoding method.

There are generally two types of PCM audio representation:

* `Float 32 Array`: It uses a 32-bit floating-point format to represent each
  sample. When capturing mic stream and setting up playback in web environment,
  PCM will be represented in this format.
* `Unsigned 8 Array`: It uses an array of 8-bit unsigned integers (aka bytes),
  and each sample can be multiple bytes. For example, for a mono PCM audio with
  bit depth of 16 bit, each sample will be two bytes. This is a lower-level
  representation and is often used in programming for audio processing.

Here's the code snippet to convert between these two formats:

```javascript theme={"dark"}
export function convertUnsigned8ToFloat32(array: Uint8Array): Float32Array {
  const targetArray = new Float32Array(array.byteLength / 2);

  // A DataView is used to read our 16-bit little-endian samples
  // out of the Uint8Array buffer
  const sourceDataView = new DataView(array.buffer);

  // Loop through, get values, and divide by 32,768
  for (let i = 0; i < targetArray.length; i++) {
    targetArray[i] = sourceDataView.getInt16(i * 2, true) / Math.pow(2, 16 - 1);
  }
  return targetArray;
}

export function convertFloat32ToUnsigned8(array: Float32Array): Uint8Array {
  const buffer = new ArrayBuffer(array.length * 2);
  const view = new DataView(buffer);

  for (let i = 0; i < array.length; i++) {
    const value = array[i] * 32768;
    view.setInt16(i * 2, value, true); // true for little-endian
  }

  return new Uint8Array(buffer);
}
```

### Audio Spec Retell AI Uses

* `Phone Calls`: Different telephony providers have different audio codecs.
  Our telephony integrations handle that for you internally, and you don't need to worry about
  encoding and decoding.
* `Web Calls`: The [frontend web JS SDK](https://www.npmjs.com/package/retell-client-js-sdk)
  abstracts away audio complexity for you. The user audio
  is captured in PCM format and sent to the backend for processing.
