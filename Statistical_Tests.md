# Explanation of Statistical Tests
The need for random and pseudorandom number arises in many cryptographic applications. For example, common cryptosystems employ keys that must be generated in a random fashion. Many cryptographic protocols also require random or pseudorandom inputs at various points, eg., for auxiliary quantities used in generateing digital signatures, or for generating challenges in authentication protocols.

There are typically two basic types of generators used to produce random sequences: random number generators and pseudorandom generators that may be used for many purposes including cryptographic, modeling and simulation applications. 

The NIST Test Suite is a statistical package consisting of 15 tests that were developed to test the randomness of (arbitrarily long) binary sequences produced by hardware or software based cryptographic random number generators or psuedorandom number generators. These tests focus on a variety of different types of non-randomness that could exist in a sequence. Some tests are deocomposable into a variety of subtests. These 15 tests are:

1. The Frequency (Monobit) Test,
2. Frequency Test within a Block,
3. The Runs Test,
4. Tests for the Longest-Run-of-Ones in a Block,
5. The Binary Matrix Rank Test,
6. The Discrete Fourier Transform (Spectral) Test, 
7. The Non-overlapping Template Matching Test,
8. The Overlapping Template Matching Test, 
9. Maurer's "Universal Statistical" Test,
10. The Linear Complexity Test,
11. The Serial Test, 
12. The Approximate Entropy Test, 
13. The Cumulative Sums (Cusums) Test, 
14. The Random Excursions Test, and
15. The Random Excursions Variant Test.

## Frequency (Monobit) Test
The focus of the test is the proportion of zeroes and ones for the entire sequence. The purpose of this test
is to determine whether the number of ones and zeros in a sequence are approximately the same as would
be expected for a truly random sequence. The test assesses the closeness of the fraction of ones to ½, that
is, the number of ones and zeroes in a sequence should be about the same. All subsequent tests depend on
the passing of this test.

### Purpose
- Checks if a string of bits (0s and 1s) is truly random.
- It does this by counting the number of 0s and 1s.
- Compare the score to a threshold

### How it works
- Count the 0s and 1s.
- Calculate a score based on the difference in counts.
- Compare the score to a threshold.

If the score is below the threshold, the sequence is considered random.

### Key Points:
- Recommended minimum sequence length: 100 bits
- Passing this test is essential for other randomness tests

## Frequency Test Within a Block
The focus of this test is the total number of runs in the sequence, where a run is an uninterrupted sequence of identical bits. A run of length k consists of exactly k identical bits and is bounded before and after with a bit of the opposite value. The purpose of the runs test is to determine whether the number of runs of ones and zeros of various lengths is as expected for a random sequence. In particular, this test determines whether the oscillation between such zeros and ones is too fast or too slow. 

### Purpose
- Checks if the number of 1s is roughly equal within smaller blocks of a long string of bits
- This is a more refined test than the Frequency (Monobit) test

### How it works
- Divide the bits into blocks of equal size.
- Count the 1s in each block.
- Calculate a score based on how different the counts are from each other.
- Compare the score to a threshold.

if the score is below the threshold, the sequence is considered random.

### Key Points:
- Recommended minimum sequence length: 100 bits
- Recommended block size: At least 20 bits, but less than 1% of the total sequence length.
- Sequence should be divided into less than 100 blocks.

## Runs Test
The focus of this test is the total number of runs in the sequence, where a run is an uninterrupted sequence
of identical bits. A run of length k consists of exactly k identical bits and is bounded before and after with
a bit of the opposite value. The purpose of the runs test is to determine whether the number of runs of ones and zeros of various lengths is as expected for a random sequence. In particular, this test determines
whether the oscillation between such zeros and ones is too fast or too slow.

### Purpose
- Checks if the number of "runs" (uninterrupted sequences of 1s or 0s) in a string of bits is as expected for a random sequence.
- Determines if the sequence is switching between 1s and 0s too quickly or slowly.

### How it works
- Count the number of runs in the sequence.
- Calculare a score based on the number of runs and the proportion of 1s in the sequence.
- Compare the score to a threshold.

if the score is below the threshold, the sequence is considered random.

### Key Points:
- Recommended minimum sequence length: 100 bits
- Prerequisite: Sequence must pass the Frequency (Monobit) test first
- Sequence with too many or too few runs will fail this test.

## Test for the Longest Run of Ones in a Block
The focus of the test is the longest run of ones within M-bit blocks. The purpose of this test is to
determine whether the length of the longest run of ones within the tested sequence is consistent with the
length of the longest run of ones that would be expected in a random sequence. Note that an irregularity in
the expected length of the longest run of ones implies that there is also an irregularity in the expected
length of the longest run of zeroes. Therefore, only a test for ones is necessary

### Purpose
- Checks if the longest runs of 1s within blocks of a string of bits are the length expected in a truly random sequence.
- Detects if 1s are clustering together too much or too little.

### How it works
- Divide the sequence into blocks of equal size.
- Find the longest run of 1s in each block.
- Count how many blocks have runs of each length.
- Calculate a score based on these counts and compare it to a threshold.

### Key Points:
- Recommended minimum sequence lengths: 128, 6272, or 750,000 bits depending on the block size used
- Block sizes: 8, 128, 104 bits 
- Sequences with too many long runs of 1s or too few will fail this test.
- Only tests for 1s, as irregularities in runs of 1s imply irregularities in runs of 0s.

## Binary Matrix Rank Test
The focus of the test is the rank of disjoint sub-matrices of the entire sequence. The purpose of this test is
to check for linear dependence among fixed length substrings of the original sequence. Note that this test
also appears in the DIEHARD battery of tests [7].

### Purpose
- Checks for linear dependence (predictable patterns) within fixed-length substrings of a bit sequence.
- Determines if bits within these substrings are too closely related with each other.

### How it works
- Divide the sequence into square matrices of equal size.
- Calculate the binary rank of each matrix (how many rows are truly independent).
- Count how many matrices have each possible rank.
- Calculate a score based on these counts and compare it to a threshold.

### Key Points:
- Recommended minimum sequence lengths: 32,912 bits (for the standard matrix size of 32x32)
- Matrix size: Typically 32x32, but can be adjusted
- Sequences with too many full-rank or too few full-rank matrices will fail this test
- Some bits at the end of a sequence might be discarded

## Discrete Fourier Transform (Spectral) Test
The focus of this test is the peak heights in the Discrete Fourier Transform of the sequence. The purpose
of this test is to detect periodic features (i.e., repetitive patterns that are near each other) in the tested
sequence that would indicate a deviation from the assumption of randomness. The intention is to detect
whether the number of peaks exceeding the 95 % threshold is significantly different than 5 %

### Purpose
- Detects periodic patterns (repetitive sequences that are close together) with a bit sequence.
- Identifies sequences that deviate from the expected randomness due to these patterns.

### How it works
- Converts 0s and 1s in the sequence to -1s and +1s.
- Applies a Discrete Fourier Transform (DFT) to identify various frequencies within the sequence.
- Calculates the heights of the peaks in the DFT results, representing the strength of different frequencies.
- Compares the number of peaks above a certain threshold to the expected number in a random sequence.
- Calculates a score based on this comparison and compares it to a threshold.

### Key Points:
- Recommended minimum sequence lengths: 1000 bits
- Sequences with too many or too few peaks above the threshold will fail this test
- Uses a mathematical technique called Discrete Fourier Transform, common in signal processing

## Non-overlapping Template Matching Test
The focus of this test is the number of occurrences of pre-specified target strings. The purpose of this
test is to detect generators that produce too many occurrences of a given non-periodic (aperiodic) pattern.
For this test and for the Overlapping Template Matching test of Section 2.8, an m-bit window is used to
search for a specific m-bit pattern. If the pattern is not found, the window slides one bit position. If the
pattern is found, the window is reset to the bit after the found pattern, and the search resumes

### Purpose
- Detects generators that produce too many or too few occurences of specific non-periodic patterns (sequences of bits that don't repeat predictably).
- Checks if certain patterns are unexpectedly common or rare.

### How it works
- Divides the sequence into blocks of equal length.
- For each block, counts how many times a specific, pre-defined pattern (template) appears.
- Compares the observed counts to the expected counts in a truly random sequence.
- Calculates a score based on this comparison and compares it to a threshold.

### Key Points:
- Recommended template length: 9 or 10 bits
- Recommended number of blocks: 8 (can be adjusted, but less than 100)
- Sequences with unusual pattern frequencies will fail this test
- Multiple templates are used to check for different patterns

## Overlapping Template Matching Test
The focus of the Overlapping Template Matching test is the number of occurrences of pre-specified target
strings. Both this test and the Non-overlapping Template Matching test of Section 2.7 use an m-bit
window to search for a specific m-bit pattern. As with the test in Section 2.7, if the pattern is not found,
the window slides one bit position. The difference between this test and the test in Section 2.7 is that
when the pattern is found, the window slides only one bit before resuming the search. 

### Purpose
- Detects generators that produce too many or too few overlapping occurrences of specific patterns (sequences of bits that overlap).
- Checks if certain patterns clump together in a ways that's unexpected in truly random sequences.

### How it works
- Divides the sequence into blocks of equal length.
- Slides a window of a specific length over each block, counting how many times a pre-defined pattern (template) appears within the window
- Compares the observed counts to the expected counts in a truly random sequence.
- Calculates a score based on this comparison and compares it to a threshold.

### Key Points:
- Slides the window by just one bit after each comparison (unlike the Non-overlapping test).
- Recommended template lengths: 9 or 10 bits
- Recommended sequence length: at least 1 million bits
- Sequences with unusual clumping of patterns will fail this test
- Specific values for degrees of freedom, block size, and template length need to be chosen carefully based on the sequence being tested

## Maurer's "Universal Statistical" Test
The focus of this test is the number of bits between matching patterns (a measure that is related to the
length of a compressed sequence). The purpose of the test is to detect whether or not the sequence can be
significantly compressed without loss of information. A significantly compressible sequence is
considered to be non-random. 

### Purpose
- Detects whether a sequence can be significantly compressed without losing information.
- Truly random sequences should not be easily compressed.

### How it works
- Divides the sequence into two segments: an initialization segment and a test segment.
- Creates a table tracking the distance betwen matching patterns (L-bit blocks).
- Examines the test segment, updating the table and calculating a sum based on the distances between match.
- Computes a test statistic (fn) based on the sum and compares it to expected values for truly random sequences.
- Calculates a P-value to determine randomness.

### Key Points:
- Focuses on distances between matching patterns.
- Uses a table to track these distances.
- Compares the observed distances to expected values for random sequences.
- Recommended sequence length: at least 387,840 bits.
- Recommended block lengths (L): 6 to 16 bits.
- Sequences that are significantly compressible will fail this test.

## Linear Complexity Test
The focus of this test is the length of a linear feedback shift register (LFSR). The purpose of this test is to
determine whether or not the sequence is complex enough to be considered random. Random sequences
are characterized by longer LFSRs. An LFSR that is too short implies non-randomness. 

### Purpose
- Detects whether a sequence is complex enough to be considered random.
- Truly random sequences should have high linear complexity (not easily generated by short LFSRs)

### How it works
- Divides the sequence into blocks of equal length (M bits).
- Calculates the linear complexity (L) of each block using the Berlekamp-Massey algorithm.
- Compares the observed L values to expected values for a truly random sequence.
- Calculates a test statistic (x2) and a p-value based on these comparisons.

### Key Points:
- Focuses on linear feedback shift register (LFSR) lengths.
- Uses the Berlekamp-Massey algorithm to compute L.
- Compares observed L values to expected distribution for random sequences.
- Recommended sequence length: at least 1 million bits.
- Recommended block lengths (M): 500 to 5000 bits.
- Sequences with low linear complexity will fail this test.

## Serial Test
The focus of this test is the frequency of all possible overlapping m-bit patterns across the entire
sequence. The purpose of this test is to determine whether the number of occurrences of the 2m m-bit
overlapping patterns is approximately the same as would be expected for a random sequence. Random
sequences have uniformity; that is, every m-bit pattern has the same chance of appearing as every other
m-bit pattern. Note that for m = 1, the Serial test is equivalent to the Frequency test of Section 2.1.  

### Purpose
- Detects whether all possible overlapping patterns of a given length (m bits) appear with approximately equal frequency, as expected in a truly random sequence.

### How it works
- Augments the sequence: Extends the sequence by appending its first (m-1) bits to the end.
- Count overlapping patterns: Counts the occurrences of all possible m-bit, (m-1)-bit, and (m-2)-bit overlapping patterns.
- Calculates test statistics: Uses the counts to compute ψ2m, ψ2(m-1), ψ2(m-2), ∇ψ2m, and ∇2ψ2m.
- Computes P-values: Uses the test statistics to calculate P-value1 and P-value2.
- Compares P-values to a threshold: if either P-value is below the threshold (e.g., 0.01), the sequence is considered non-random.

### Key Points:
- Focuses on overlapping patterns of a specific length (m bits).
- Compares observed pattern frequencies to expected frequencies for random sequences.
- Recommended sequence length: at least 1000 bits.
- Recommended block length (m): less than log2(n) - 2.
- Sequences with uneven pattern frequencies will fail this test.

## Approximate Entropy Test
As with the Serial test of Section 2.11, the focus of this test is the frequency of all possible overlapping
m-bit patterns across the entire sequence. The purpose of the test is to compare the frequency of
overlapping blocks of two consecutive/adjacent lengths (m and m+1) against the expected result for a
random sequence.   

### Purpose
- Detects whether a sequence has the appropriate level of randomness or irregularity by comparing the frequency of overlapping blocks of two consecutive lengths (m and m+1)
- Truly random sequences should have a high approximate entropy (ApEn), indicating substantial fluctuation or irregularity.

### How it works
- Augments the sequence: Extends the sequence by appending its first (m-1) bits to the end, for both block lengths (m and m+1).
- Count overlapping blocks: Counts the occurrences of all possible m-bit and (m+1)-bit overlapping patterns.
- Calculates ϕ(m) and ϕ(m+1): Uses the counts to compute ϕ(m) and ϕ(m+1), which are measures of the "complexity" of the patterns.
- Computes ApEn(m): ApEn(m) = ϕ(m) - ϕ(m+1), representing the difference in complexity between the two block lengths.
- Calculates χ2 statistic: Uses ApEn(m) to compute the χ2 statistic.
- Computes P-value: Uses the χ2 statistic to calculate a P-value.
- Compares P-value to a threshold: If the P-value is below the threshold (e.g., 0.01), the sequence is considered non-random.

### Key Points:
- Focuses on comparing pattern frequencies of two block lengths.
- High ApEn indicates randomness, low ApEn indicates regularity.
- Recommended sequence length: at least 100 bits.
- Recommended block length (m): less than log2(n) - 5.
- Sequences with too much regularity will fail this test.

## Cumulative Sums (Cusum) Test
The focus of this test is the maximal excursion (from zero) of the random walk defined by the cumulative
sum of adjusted (-1, +1) digits in the sequence. The purpose of the test is to determine whether the
cumulative sum of the partial sequences occurring in the tested sequence is too large or too small relative
to the expected behavior of that cumulative sum for random sequences. This cumulative sum may be
considered as a random walk. For a random sequence, the excursions of the random walk should be near
zero. For certain types of non-random sequences, the excursions of this random walk from zero will be
large. 

### Purpose
- Detects whether the cumulative sum of 1s and 0s in a sequence exhibits excessive deviations from zero, which would indiciate non-randomness.
- True random sequences should have cumulative sums that stay relatively close to zero.

### How it works
- Normalize the sequence: Converts 0s to -1s and 1s to +1s.
- Calculates partial sums: Computes sequential sums of the normalized values, starting either from the beginning (forward) or the end (backward) of the sequence.
- Finds the maximum excursion: Identifies the largest absolute value among all the partial sums.
- Calculates P-values: Uses the maximum excursion to computer a P-value based on the normal distribution.
- Compares P-value to a threshold: If the P-value is below the threshold (e.g., 0.01) the sequence is considered non-random

### Key Points:
- Focuses on the cumulative sum of 1s and 0s.
- Large deviations from zero indicate non-randomness.
- Recommended sequence length: at least 100 bits.
- Applies in both forward and backward directions.
- Sequences with uneven distribution of 1s and 0s will fail this test.

## Random Excursions Test
The focus of this test is the number of cycles having exactly K visits in a cumulative sum random walk.
The cumulative sum random walk is derived from partial sums after the (0,1) sequence is transferred to
the appropriate (-1, +1) sequence. A cycle of a random walk consists of a sequence of steps of unit length
taken at random that begin at and return to the origin. The purpose of this test is to determine if the
number of visits to a particular state within a cycle deviates from what one would expect for a random
sequence. This test is actually a series of eight tests (and conclusions), one test and conclusion for each of
the states: -4, -3, -2, -1 and +1, +2, +3, +4.

### Purpose
- Focuses on the number of times a random walk visits specific states (values) within cycles.
- Detects non-random patterns in the number of visits to each state.
- True random walks should have visit frequencies that align with theoretical expectations.

### How it works
- Converts the input sequence to a random walk: Transforms 0s and 1s into -1s and +1s, creating a sequence of steps.
- Identifies cycles: Finds sequences of steps that start and end at zero (the origin).
- Counts state visits within cycles: For each state (-4 to +4) counts how often its visited within each cycle
- Calculates chi-square statistics: Compares the observed visit counts to expected counts for a truly random walk.
- Computes P-values: MEasures the probability of seeing the observed counts if the sequence were random.
- Compares P-values to a threshold: If P-values are below a threshold (e.g., 0.01), the sequence is considered non-random for that state.

### Key Points:
- Focuses on state visits within cycles of a random walk.
- Compares observed visit counts to theoretical expectations.
- Uses chi-square statistics and P-values to assess randomness.
- Recommended sequence length: at least 1,000,000 bits.
- Tests for non-randomness in each of 8 states (-4 to +4).
- Contradictory results (some states random, some non-random) might require further investigation.

## Random Excursions Variant Test
The focus of this test is the total number of times that a particular state is visited (i.e., occurs) in a
cumulative sum random walk. The purpose of this test is to detect deviations from the expected number
of visits to various states in the random walk. This test is actually a series of eighteen tests (and
conclusions), one test and conclusion for each of the states: -9, -8, …, -1 and +1, +2, …, +9. 

### Purpose
- Focuses on the total number of times each state is visited in a random walk, not just within cycles.
- Detects deviations from expected visit frequencies across the entire walk.
- True random walks should have visit counts that align with theoretical expectations.

### How it works
- Converts the input sequence to a random walk: Transforms 0s and 1s into -1s and +1s, creating a sequence of steps.
- Counts state visits across the entire walk: Records how often each state (-9 to +9) is visited.
- Calculates test statistics: Compares observed visit counts to expected counts for a truly random walk.
- Computes P-values: Measures the probability of seeing the observed counts if the sequence were random.
- Compares P-values to a threshold: If P-values are below a threshold (e.g., 0.01), the sequence is considered non-random for that state.

### Key Points:
- Focuses on total state visits across the entire random walk.
- Uses half-normal distribution for P-values.
- Recommended sequence length: at least 1,000,000 bits.
- Tests for non-randomness in each of 18 states (-9 to +9).
- Conclusions are drawn for each state individually.