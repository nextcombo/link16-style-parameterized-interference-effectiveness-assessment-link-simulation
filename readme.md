# Link16 style parameterized interference effectiveness assessment: link simulation, multi-index construction and combined weighted scoring[with python code]

Link16 Style Parameterized Interference Efficacy Assessment: Link Simulation, Multi-Metric Construction, and Compositional Weight Scoring Summary
This paper constructs a Link16 style parameterized interference efficacy assessment framework for public research. The framework consists of three levels: (1) Physical layer link simulation - establishing a complete transmission and reception link from information bits to radio frequency transmission, and injecting three types of parameterized interference profiles into the channel; (2) Four module eleven indicator system - covering four functional modules, decoding, dehopping, demodulation, and decoding, respectively extracting the degradation quantities of signal level and decoder level anti-interference performance.(3) Combining Weighted Factors with Grey Relational Ranking - By employing a game theory combination of AHP (Analytic Hierarchy Process) subjective weighting and D-CRITIC objective weighting, continuous scoring labels are produced within the framework of grey relational analysis. This paper provides a rigorous derivation of the complete data generation pipeline, indicator definitions, mathematical formulas, and scoring methodology. Key terms: Link16; Frequency Hopping Spread Spectrum; Parametric Interference; Efficacy Assessment; D-CRITIC; Game Theory Combining Weighted Factors; Grey Relational Analysis
1 Introduction The quantitative evaluation of the performance of tactical data chain in jamming environment is a core topic in communication anti-jamming research and electronic countermeasures effectiveness analysis. However, existing public evaluation datasets often have two problems:
The evaluation indicators are too single (usually only recording the bit error rate), which can not reflect the differentiated damage of each link.
The scoring method lacks systematic integration of subjective and objective elements, and the differentiation and interpretability of labels are insufficient.
The proposed parameterized interference effectiveness assessment framework, using the Link16 style physical layer link as the simulation object, can be summarized into four core contributions:✅ Completely frozen transmission and reception links: covering CRC, RS encoding/decoding, block interleaving, CCSK spread spectrum despreading, MSK modulation/demodulation, dual-pulse frame combining, and pseudorandom hopping across the entire process;✅ Three types of parameterized interference profiles: broadband overlap interference, comb-like partial frequency band interference, and sweeping partial frequency band interference, characterized by frequency coverage items, density shape items, and time overlap coefficients;✅ Four module 11 indicator system: defining 11 indicators with clear physical meanings from four dimensions: demodulation, dehopping, despreading, and decoding, distinguishing between formal definitions and simulation implementation criteria.✅ Combination of weighting and GRA endpoint scoring: AHP and D-CRITIC implement a combination of subjective and objective integration through game theory, combining gray correlation endpoint ratio to produce continuous scoring.
2. System Model
2.1 Overall Link Architecture: The system simulates a closed-loop loop of "Information bits → Transmission link → Channel → Received link → Information bits". Each scenario is compared using the paired-run method (only AWGN for reference links, and AWGN + parameterized interference for interfered links). Performance degradation metrics are extracted between the two signals in the middle.
Transmission chain process: Information bits → CRC-16-CCITT → RS (31,15)×4 → 23×4 block interleave → 5-bit packet → CCSK (5,32) → MSK coded waveform → Dual pulse frame → Frequency hopping shift
Receiver chain process: Radio frequency front end → De-hop → Dual pulse combining → MSK coded demodulation → CCSK correlation decision → 5-bit de-packet → De-interleave → RS decoding → CRC check
2.2 Key Link Parameters
Numeric Value
Meaning
Information bit / packet
284 bit
Per packet information load
CRC
CRC-16-CCITT
Polynomial
0x1021
RS Code
(31,15), GF(2⁵)
Per packet 4 blocks, correctable symbols per block/packet
Interference
23 × 4 matrix transpose
Block size
92 bit
CCSK
(5,32)
5-bit → 32-chip codeword
Sampling rate (normalized)
1.0
Each chip 8 sampling points
Dual pulse
528 samples/frame
Interval 2 chips
Frequency hopping
51 frequency slots, every 4 frames one hop
Slot interval 0.80/50
3. Transmission Link
3.1 CRC-16-CCITT Information Bit Sequence is encoded by a polynomial (0x1021) to generate 16-bit check bits, which are then combined to form a 300-bit sequence with a check sequence.
3.2 RS (31, 15) encoding constructs Reed-Solomon codes over a finite field:
The 300-bit sequence is divided into 60 symbols of 5-bit groups, equally distributed into 4 RS blocks (each containing 15 message symbols);
Each block is expanded to 31 symbols, capable of correcting 8 symbol errors;
The total number of codeword bits after encoding: bits.
3.3 Block Interleaving: The 620-bit codeword is divided into 92-bit blocks, each reshaped into a 23×4 matrix, and outputted in a "row write, column read" manner, spreading burst errors across multiple codewords to enhance the effectiveness of RS decoding.
3.4 CCSK (5, 32) spread spectrum core logic: A 5-level LFSR is used to generate a 31-bit m sequence, with the first bit of the double-polarity codeword repeated after mapping to bipolar code chips, resulting in a 32-bit reference codeword; cyclic shifting generates a codebook of 32 codewords, each corresponding to one 5-bit symbol.
3.5 MSK modulation employs minimum frequency shift keying (constant envelope continuous phase modulation), with each code chip having a phase increment of , ensuring phase continuity and constant envelope, taking 8 sample points per code chip.
3.6 Dual pulse frame formation: Each CCSK symbol generates 256 sample point pulses, duplicated into two segments and inserted with 16 sample point zero-padding gaps, resulting in a frame length of 528 sample points after power normalization.
3.7 Frequency hopping system uses 51 equally spaced frequency slots, with a pseudorandom sequence (4 hops per 4 frames) for the frequency hopping pattern, whereby the transmitter generates phase rotation through frequency accumulation to achieve frequency hopping.
4. Channel and Interference Model
4.1 AWGN Additive White Gaussian Noise is an independent identically distributed complex Gaussian random variable: (, I/Q components independently follow).
4.2 Parametrically-induced interference overall framework: The interference signal is controlled by four parameters: "Profile type, Signal to Noise Ratio (JSR), Spectral occupation, and time overlap". The generation process is as follows: original waveform → frequency envelope → shaped waveform → power normalization → JSR amplitude adjustment → time gate control → final interference.
Reference / Interference reception signal formula:
4.3 Total interference budget: The total interference power is determined by the JSR (dB): (JSR range: -19~-10 dB); each hit frequency slot power budget: corresponding injection amplitude.
4.4 Frequency domain characterization defines two core factors:
Coverage item: describes the presence of interference on frame frequency slots;
Density item: describes the concentration of interference energy within hit frequency slots;
Frequency overlap weight:
4.5 Three types of interference profiles:
Frequency domain characteristics
Core waveform
Bandwidth overlap interference
Center frequency slot + radius coverage, density item is negatively correlated with spectral occupation
Complex Gaussian white noise through 129 Hanning window filtering
Comb-like frequency band interference
Select high-frequency hopping frequency slots, coverage item is 0/1 binary
Carrier tracking hopping + four-phase random code scrambling
Sweep frequency band interference
Center frequency linear scanning, coverage item contains time focusing factor
Sweep frequency chirp superimposed phase jitter
4.8 Time overlap: The time overlap coefficient, interference only activates in the central continuous segment of the signal's entire length, final interference signal:
5.1 Dehop receiving end uses known hopping pattern conjugate derotation:
5.2 Dual pulse combining after splitting into frames, adds weighted pulses together, maintaining unit power matching after combination.
5.4 CCSK correlation detection soft decision code chips are correlated with CCSK codebook, taking the maximum correlation value index as the detection symbol, core indicators:
Peak sidelobe ratio (PSR): measures the spread spectrum discrimination gain;
Symbol error rate (SER): the proportion of despread symbol errors.
5.5 Interleave and RS decoding detection symbol recovery: After deinterleaving and RS decoding, the symbols are recovered from the bitstream. The RS decoding is performed separately for four blocks, defining "RS block success rate (BSR)": The proportion of correctly decoded blocks in four blocks.
6 Core design principles of the 4-module indicator system, except X1-X3 (demodulation offsets), X8 (white noise similarity), all other indicators adopt the "loss metric paradigm": To prevent zero division.
6.1 Demodulation module (X1-X3): Absolute difference between the amplitude offset of the disturbed / reference signal and the normalized amplitude variance;
X2 Frequency offset: Absolute difference between the frequency characteristics of the disturbed / reference signal (wavelet detail coefficient energy mean);
X3 Phase offset: Standard deviation of phase difference between strong signal regions, normalized to [0,1].
6.2 Dehop module (X4-X5): Mean ratio of active signal area interference power to total power;
X5 Frequency overlap degree: Weighted mean of interference power proportion based on frequency overlap weight.
6.3 Despreading module (X6-X8): Loss measure based on PSR (1 - disturbed PSR / reference PSR);
X7 Error correction capability: Loss measure based on CCSK symbol correctness rate;
X8 White noise similarity: Spectral flatness of residual signal after disturbance (closer to 1, more like white noise).
6.4 Decoding module (X9-X11): Interleave degree: Loss measure of interleave gain (interleave gain = 1 - error burstiness with or without interleaving);
X10 Error correction capability: Loss measure based on RS block success rate;
X11 Coding efficiency: Loss measure of effective information ratio after encoding.
7 Normalization of Indicators 7.1 Half saturation index mapping (X1, X2): Unbounded original values mapped to [0,1), where the median of 324 calibrated scenarios is determined (ensuring that the median mapping results in 0.5).
7.2 Truncation normalization (X3-X11) X3:;
X4-X7, X9-X11: directly truncate to [0,1]
X8: Natural [0,1], keep the original value.
8. Scene Generation and Data Set Construction
Factor
Value
Quantity
Cross-section type
Bandwidth overlap / comb / sweep frequency
3  
Dry-to-wet ratio (JSR)
-19~-10 dB (step 1)
10  
Time overlap
1/3, 2/3, 1  
3  
Spectrum occupation
17, 34, 51 Frequency slots
3  
Random seed
1~20  
20  
8.2 Single-scene Simulation Process 1
Generating information bit sequences based on seeds;
2  
Execute paired-run (interleaved on/off, shared random instances);
3  
Generate reference / interference reception signals;
4  
Calculate X9 interleave gain (burst stress detection);
5  
Extract intermediate signals at each level of the link;
6  
Generate 11-dimensional raw indicators, and normalize to obtain the evaluation indicator vector.
9 v3 Chain of scoring tags from 11-dimensional metrics to a single continuous score: refers to
Specs
Main
Observation
Rights
Reload
Bao
Yi
On
Group
Compliance
Group
Compliance
Rights
Reload
Most
End
Evaluation
Divided
9.1 AHP Subjective Weighting Two-Level Structure: Top-level (4 modules) + Sub-level (indicators within each module);
Module Importance: Modulation > Frequency Hopping > Spread Spectrum > Coding;
Conformity inspection: Only when CR < 0.10 is it considered to meet the requirements.
9.2 Robust normalization replaces mean / standard deviation with median, absolute deviation (MAD), and combines tanh function to limit the range:
9.3 D-CRITIC Objective Weighting Core Improvement: Replacing Pearson correlation coefficient with distance-related coefficient (capturing nonlinear dependencies) to calculate the information content of indicators and weights, and assessing weight stability through Bootstrap.
9.4 Game Theory Compositional Weighting Based on Subjective/Objective Weights Reliability, minimizing weighted deviation to obtain compositional weights: (, determined by reliability weighting).
9.5 GRA Grey Relational Scoring 1
Construct a 5-level reference sequence (0 = low damage, 4 = high damage);
2
Calculate grey relational coefficient and degree of correlation (compositional weighting);
3
End-point ratio scoring:;
4
Normalize to obtain final scores (larger indicates more severe disturbance damage).
10 Conclusion The Link16 style parameterized interference efficacy assessment framework proposed in this paper, core innovations can be summarized as:
1
Full-process link freezing: A closed link from CRC to frequency hopping, ensuring unified assessment baseline;
2
Parameterized interference profile: Three types of profiles cover typical interference strategies, parameters are controllable;
3
Multidimensional index system: Four modules with eleven indicators, clear physical meanings, quantifying damage at various links in the chain;
4
Combination of subjective and objective scoring: D-CRITIC captures nonlinear dependencies, game theory compositional weighting, and GRA scoring distinguishes higher.
This framework has a rigorous mathematical definition and traceable engineering implementation, providing a complete theoretical and methodological foundation for research on interference efficacy assessment and dataset construction.
References[1] Saaty, T. L. The Analytic Hierarchy Process. McGraw-Hill, 1980.[2] Diakoulaki, D., et al. "Determining Objective Weights in Multiple Criteria Problems: The CRITIC Method." Computers & Operations Research, 1995.[3] Székely, G. J., et al. "Measuring and Testing Dependence by Correlation of Distances." The Annals of Statistics, 2007.[4] Deng, J. L. "Introduction to Grey System Theory." The Journal of Grey System, 1989.[5] Proakis, J. G. Digital Communications, 5th ed. McGraw-Hill, 2008.[6] Simon, M. K. Bandwidth-Efficient Digital Modulation. Wiley, 2003.  

## Images

![Link16 style parameterized interference effectiveness assessment: link simulation, multi-index construction and combined weighted scoring [with python code]](01.jpg)
![Link16 style parameterized interference effectiveness assessment: link simulation, multi-index construction and combined weighted scoring [with python code]](02.jpg)
![Link16 style parameterized interference effectiveness assessment: link simulation, multi-index construction and combined weighted scoring [with python code]](03.jpg)
![Link16 style parameterized interference effectiveness assessment: link simulation, multi-index construction and combined weighted scoring [with python code]](04.jpg)
![Link16 style parameterized interference effectiveness assessment: link simulation, multi-index construction and combined weighted scoring [with python code]](05.jpg)

<!-- PAYMENT_BLOCK -->

## Get this dataset

- Payment link (Stripe): [Buy now](https://buy.stripe.com/3cs8yP7sY87d0vu9AB)
- Payment QR code: ![Payment QR code](pay.png)

<!-- /PAYMENT_BLOCK -->
