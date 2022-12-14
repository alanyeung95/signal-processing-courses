Here is my cert:

<img src="cert.jpeg" alt="drawing" width="1000"/>

<br>

But more importantly, below are my notes on this course:

# section-1

## Amplitude and Frequency

- Amplitude: distance between the resting position and the maximum displacement of the wave.
- Frequency: number of waves passing by a specific point per second.

# section-2: time series denoising

## time series filter :star:

if k (windows size) is very large, edge effect will be large also as data between 0 to k will be 0

### mean-smoothing

### gaussian-smoothing

<img src="diagrams/diagram-3.png" alt="drawing" width="400"/>
<img src="diagrams/diagram-4.png" alt="drawing" width="400"/>

- more smoother then mean-smooth
- data near windows center has more weighting
- k should be minimial as possible (to avoid edge effect) while the first item of gaussian window should be closed to 0

### mean-smoothing vs gaussian-smoothing

<img src="diagrams/diagram-5.png" alt="drawing" width="400"/>

### electromyogram

- denoising via TKEO
- convert original signal and TKEO signal as `z score`
- z-score: standard score is the number of standard deviations a given data point lies above or below the mean. To calculate the Z-score, subtract the mean from each of the individual data points and divide the result by the standard deviation. Results of zero show the point and the mean equal. A result of one indicates the point is one standard deviation above the mean and when data points are below the mean, the Z-score is negative. In most large data sets, 99% of values have a Z-score between -3 and 3, meaning they lie within three standard deviations above or below the mean.

### median filter :star:

- use histrogram to pick threshold
- compare with mean filter it preserve the detail of datas (e.g. the edge on the graph)

## data processing

### linear detrending

### nonlinear trend with polynomials (modle fitting)

- use `Bayes information criterion` to find optimal order

# section-3 spectral and rhythmicity analyses :star:

## time domain vs frequency domain

<img src="diagrams/diagram-1.png" alt="drawing" width="400"/>

## terms

- sampling rate: the number of samples per second (or per other unit) taken from a continuous signal to make a discrete or digital signal.

- Nyquist frequency: In units of cycles per second (Hz), its value is one-half of the sampling rate (samples per second)

## fourir transform

### convert signal from time domain to frequency domain

- some signal are better understood in the frequency doamin
- spectral analyses can reveal insights that cannot be seen in the time domain

### other goals

- frequency domain computations are often easier and faster than time domain
- use the convolution theorem to perform other operation in the frequency domain like filtering, autocorrection, etc.

### Welch's method

- divide time into different block (e.g. n time block)
- compute the frequency and power for each time block
- later can use the data to draw a spectrogram (with n time block)

# section-4: complex numbers

## Complex conjugate

In mathematics, the complex conjugate of a complex number is the number with an equal real part and an imaginary part equal in magnitude but opposite in sign. That is, the complex conjugate of `a+bi` is equal to `a-bi`

# sectoin-5: filtering :star:

## fitering in frequency domain :star:

1. data in time domain convert to frequency domain and cut-offs (high pass(perserve high frequency only) or low pass filtering)
2. convert signal from frequency to time domain using `inverse fourier transform`.
   <img src="diagrams/diagram-7.png" alt="drawing" width="600"/>

## filter time domaiain :star:

1. signal with noises
2. for each value in time point, apply filter kernel on signal
   <img src="diagrams/diagram-8.png" alt="drawing" width="600"/>

## filter time series steps :star:

1. define frqeuency-domain shape and cut-offs
2. generate filter kernel (firls, fir1, butter or other)
3. evaluate kernel and its power spectrum
4. apply filter kernel to data
   <img src="diagrams/diagram-9.png" alt="drawing" width="600"/>

## taper function

1. Hann vs Hamming vs Gauss

- Hann and Hamming are similiar, but Hann will touch `0`. So Hamming may still have edge effect.
- Gauss windows are narrower and will go quickier to `0`

<img src="diagrams/diagram-6.png" alt="drawing" width="700"/>

## FIR vs IIR :star:

|               | FIR                       | IIR                                                                              |
| ------------- | ------------------------- | -------------------------------------------------------------------------------- |
| Name          | Finite impulse response   | Infinite impulse response                                                        |
| Kernel length | Long                      | Short                                                                            |
| Speed         | Slower                    | Fast (better for online/real time filtering because of short kernel length)      |
| Stability     | High                      | Data-dependent                                                                   |
| Mechanism     | Multiply data with kernel | Multiply data with data (if previous data is wired, future data may be affected) |

## FIR

### order

The order of the filter is the number of time points in the filter. So an order-13 mean-smoothing filter has 13 points, which means that each point t in the filtered signal is the average of points t-6 to t+6. For example, variable k to be how far back/forwards to go, thus if k=6 then the order is 13.

## IIR

use 2 kernel filters.

1. weigths for the previous values of the original signal,
2. previous values of the already filtered signal

### Zero-phase filtering

Zero-phase filtering is a non-causal procedure, so it cannot be done in real time, only offline (or pseudo real-time, i.e., with a sufficient delay). A zero-phase filter needs to have a purely real-valued frequency response

## remove edge effect

clone the orignal signal, then add it to the start and the end of the original signal

## low-pass filtering

### FIR and IIR

1. define frequency cut-off and transition windows

### windowed sinc function

- supress faster after target frequencies
- has round shape later and flat line at the end

## narrow-band filter

- make sure the lower band and upper band have same size of transition windows

## two-stage wide-band filter

- filter as two step if one time filter cannot create a good filter

# section-6: Convolution

Definition:
Convolution is a mathematical way of combining two signals to form a third signal. Using the strategy of impulse decomposition, systems are described by a signal called the impulse response. Convolution is important because it relates the three signals of interest: the input signal, the output signal, and the impulse response.

A way to combine two time series (or images...)

- Signal: the "interesting" time series
- Kernel: The filter
- Convolution result: A mixture of the features of the signal and the kernel

## importance :star:

- can be used for better denoising/filtering (e.g. narrowband filtering), it is because it doesn't have edge effects show in time domain
- provide new way of looking at signal (2 signals become a new signal)
- computation convolution in frequency domain is easier than in time domain

## convolution theorem :star:

[<img src="https://dartbrains.org/_images/ConvolutionTheorem.png">](https://dartbrains.org/_images/ConvolutionTheorem.png)

- convolution: signal & kernal in time domain -> the result of convolution -> fft -> signal in frequence domain
- signal -> fast fourier transform -> fourier spectrum -> multiply each frequency of signal and corresponding kernel -> signal in frequency domain -> ifft -> the result of convolution

### why fast fourier fransform approach is recommanded

- fast fourier transform is really fast compare with time domain convolution if the time series is long
- fast fourier transform require fewer lines of code

## convolution result = dot product of data and kernel

suppose kernel lenght = 3
convolution [i] = data[i-1:i+2] dot kernel[i-1:i+2]

length of convolution result = length of data + length of kernel - 1

## using morlet wavelet as convolution filter

<img src="diagrams/diagram-2.png" alt="drawing" width="700"/>

## Planck-taper

[<img src="https://www.recordingblogs.com/rbdocs/wiki/plancktaperwindow-window.png">](https://www.recordingblogs.com/rbdocs/wiki/plancktaperwindow-window.png)

- different from Gaussian function, which has one peak point but Planck-taper can be defined with many peak point.
- 0<?????0.5 (determine steepness of increase and decress)

# section-7: Wavelet-analysis

Wavelets are functions that you can use to decompose signals.

## application of wavelets :star:

1. filtering (fime-frequency analysis)
2. feature detection (pattern-matching).
   1. e.g., go through a time series and use a wavelet as a template to search for a particular feature embedded in the signal

feature delection of wavelet in time domain analysis
<img src="diagrams/diagram-10.png" alt="drawing" width="700"/>

## species of waveletes

1. Morlet wavelet
   1. need to normalize the filter signal manually as filtered signal. As the amplitude is hundreds time larger than the oringal one
2. Haar wavelet (could be used as edge detector in time domain, not useful in frequency domain/ specturm analysis)
   1. non negative value are on the right hand side of the Haar function, that's why the peak of Haar convolution will be on the right of Mexican function
3. Mexican wavelet
4. Difference of Gaussian (DoG) wavelet

- different wavelet have different applications in signal processing. e.g., morlet wavelet are useful for time frequency analysis because there are nicely localized in the frequency domain

## complex wavelet

consist with real part and imaginary part

### complex plane

[<img src="https://qph.cf2.quoracdn.net/main-qimg-7795d2cc61760acdcded37a2bc96867c-lq">](https://qph.cf2.quoracdn.net/main-qimg-7795d2cc61760acdcded37a2bc96867c-lq)

- in a imag & real plane, distance from the origin is the `magnitude`
- `phase angle` = ??
- Real-valued wavelets are used for filtering, complex-valued wavelets are additionally used for time-frequency analysis (e.g., power and phase).

# section-8: Resampling

## interpolation/resampling/upsampling: insert extra points/data

## downsampling

steps:

1. pick new sampling rate
2. low-pass filter at new Nyquist
3. downsample

## how to resample in matlab?

for example from 100 to 80 factor

first upsample to 400 by 4 then downsample to 80 by factor of 5

## interpolation :star:

linear, next, nearest, spline (better in frequency domain as the number of sharp edges)

## extrapolation

linear vs spline/cubic (data generated will be more exaggerated when compares with linear appraoch)

## spectral interpolation (what to do when a large chunk of time signal is missing?) :star:

interpolation data in time domain

steps:

1. it is suggested that the windows size before and after are the same
2. convert 2 windows from time domain to frequency domain
3. create the third frequency by averaging 2 spectral
4. using linear trend to connnect 2 points smoothly (detrend mock data, and add the slope from 2 window points)

## Dynamic time warping

dynamic time warping (DTW) is an algorithm for measuring similarity between two temporal sequences

## section-9: Outlier detection

outliner effect: pull the mean up

## deal with outlier

- remove outliner with standard deviation, then interpolate the missing points
- detect outliner: global or local (with window size as percent of total signal length) threshold

## window outlier

use Root mean square, find all local RMS, then pick the threshold manually

# sectoin-10 Feature detection

## find local minima/maxima

`peeks1 = np.squeeze( np.where(np.diff(np.sign(np.diff(signal1)))<0) )+1`

## recover signal/feature from noise amplitude

four approach

- Rectify
- Hilbert
- TKEO
- Variance

also all approach need parameter `k` (window size) to do smoothing

## Wavelet convolution for feature extraction

identify specific feature on signal, assume you have the template of the event/feature, but don't know when it will happen in the real data

steps:

1. calculate convolution between data and the event template
2. identify pattern and pick threshold
3. pick events

## EMG data feature detection

use TKEO algorithm and threshold to capture first spike of the signal

## full width half maximum

The full width at half maximum (FWHM) is a parameter commonly used to describe the width of a "bump" on a curve or function. It is given by the distance between points on the curve at which the function reaches half its maximum value.

it is used to measure the shape of the dataset

### steps to get full width half maximum:

1. define a gaussian function with sampling rate
2. find peak point
3. then find 50% th of the point from start to peak
4. then find 50% th of the point from peak to end

better to smooth the curve before finding the full width half maximum

### empirical value

The empirical rule is a quick way to get an overview of your data and check for any outliers or extreme values that don???t follow this pattern.

The empirical rule, or the 68-95-99.7 rule, tells you where most of the values lie in a normal distribution:

```
Around 68% of values are within 1 standard deviation of the mean.
Around 95% of values are within 2 standard deviations of the mean.
Around 99.7% of values are within 3 standard deviations of the mean.
```

## area under the curve

https://byjus.com/area-under-the-curve-formula/#:~:text=The%20area%20under%20a%20curve,using%20integration%20with%20given%20limits.

# section-11 Variability

## two approach to measure variability of data

1. Root Mean Square
2. Variance (substract the mean)

### SNR (signal to noise ratio) :star:

SNR compares the level of a desired signal to the level of background noise. SNR is defined as the ratio of signal power to the noise power, often expressed in decibels.

### two approach to increase SNR

1. boost signal
2. reduce noise

## Coefficient of variation

formula: std/mean
