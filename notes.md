# section-1

## Amplitude and Frequency

- Amplitude: distance between the resting position and the maximum displacement of the wave.
- Frequency: number of waves passing by a specific point per second.

# section-2

## time series filter

if k (windows size) is very large, edge effect will be large also as data between 0 to k will be 0

### mean-smoothing

### gaussian-smoothing

- more smoother then mean-smooth
- data near windows center has more weighting
- k should be minimial as possible (to avoid edge effect) while the first item of gaussian window should be closed to 0

### electromyogram

- denoising via TKEO
- convert original signal and TKEO signal as `z score`

### median filter

- use hist to pick threshold
- compare with mean filter it preserve the detail of datas (e.g. the edge on the graph)

## data processing

### linear detrending

### nonlinear trend with polynomials (modle fitting)

- use `Bayes information criterion` to find optimal order

# section-3

## time domain vs frequency domain

![alt](diagrams/diagram-1.png)

## fourir transform

### convert signal from time domain to frequency domain

- some signal are better understood in the frequency doamin
- spectral analyses can reveal insights that cannot be seen in the time domain

### other goals

- frequency domain computations are often easier and faster than time domain
- use the convolution theorem to perform other operation in the frequency domain like filtering, autocorrection, etc.
