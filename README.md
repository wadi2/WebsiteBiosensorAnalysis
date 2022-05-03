# WebsiteBiosensorAnalysis

## UW-Madison CS766 Computer Vision Spring 2022 Project Proposal	 

### Team members: Michael Svidro Nelson (msnelson8@wisc.edu), Andrew George Negrut (anegrut@wisc.edu), and Wihan Adi (wadi2@wisc.edu)  
 
### Code can be found at: https://github.com/wadi2/BiosensorDataAnalysis
 
# Introduction 

Refractometric index sensing for biological applications using photonics nanostructures is an emerging method for developing affordable point-of-care diagnostics [1–5]. Leveraging different types of physical phenomena, the presence of target biomolecules can be detected down to a very low concentration (for example as low as 1 pg/mL [3]) using an optical reader that is very similar to a brightfield microscope.  

  

Fig.1 Illustration of bio-sensing using an imaging-based nanostructure. The acquisition of hyperspectral data cube is usually done in transmission mode, where multiple capture molecules (proteins) are immobilized (a). The image of the protein microarray spots before and after incubation of liquid sample (b). If the corresponding target molecules are present, the spectral signal at the protein spot changes; for example, as shown on the right (b). Given that the data is a hyperspectral data cube, each pixel contains a spectral signal and the average spectral signal of protein A before and after incubation are shown in (c). 

 

Even as the physical mechanism behind the detection method varies, most research and the deployment of such sensors require similar types of image analysis, which will be elaborated further below. Fig 1. (a) shows one variation of such a sensor, and the detection principle goes as follows: firstly, biological capture molecules, such as antibodies, (any protein spots shown in Fig. 1 (a)) are immobilized on top of the sensor surface. Subsequently, a hyperspectral data cube of the chip is taken (fig. 1 (b), left). Then, a sample liquid e.g. serum, saliva, etc., are placed on top of the chip. After some incubation time, the liquid samples are removed and the hyperspectral data cube of the same chip is taken again. If the target molecule was present within the sample liquid, the sensor will show a different spectral response within a given sample spot (Fig. 1(c)). In order to quantitatively assess how much target molecule was present, the wavelength shift of the peak maxima, Δλ, or the change in intensity at a given wavelength, ΔI(λp), is calculated.  Extracting either piece of information requires comparing the before and after picture of a particular sample, aligning the regions of interest, and dealing with any artifacts, missing spots or partially missing spots.  

![Figure 1](WebsiteBiosensorAnalysis/docs/assets/fig1.png)


# The Challenge and Problem Importance 

As mentioned, concentrations of target molecules could be obtained through comparisons between the images/hyperspectral data cubes of a region of interest before and after incubation with a given sample. However, since the samples are physically removed from the imaging instrument for incubation between data collection sessions, there is always bound to be some misalignment (Fig. 2), however miniscule. One way to realign the image is to use a fiducial marker such as the control protein (protein C in Fig. 1). Even with the control as a fiducial marker; however, there remains the problem of how to handle spots that are partially or entirely missing due to misalignment of the sample and field of view. The goal is quantitative analysis of intensity changes related to target molecule binding, and so precise measurements and comparisons are important. 

  
Moreover, given that there are multiple target molecules of interest (3 in this example but the number is arbitrary), there is a need to quickly extract data at multiple locations in two images. Ideally, the solution would also be interactive to enable rapid prototyping during research phase.  

  

 

Fig 2. FOV of before (left), and after (right) incubation. Each column represents protein A, protein B, and protein C. Two repeats of each proteins are shown here and are divided by a blank spot.  

 

A further advantage of this approach is that, in addition to detecting multiple proteins, the image capture protocol could be streamlined for spectrometer-less detection. In the spectrometer-less method, instead of using a tunable wavelength source, the nanophotonic device is tuned to give spectral information using a fixed wavelength source (illustrated in Fig. 4). There, similar to the biological multiplexing, the spectral information is encoded in the spatial variation of the nanophotonic structure. The development of these devices and protocols would greatly benefit from an intuitive and interactive interface to create masks as required by experimental conditions and needs. 

 

Fig. 5. The use of different nanophotonics structures at different spatial location for spectrometer-less biosensing. 

 

# Proposed Work  

We will use existing image analysis libraries available in the Python programming language to align a pair of before and after hyperspectral data cubes, given some fiducial marker (protein C). The differences between intensities per wavelength will then be extracted for visualization in the context of the experimental design – being matched up with their appropriate experimental label and possibly wavelength of interest. The dataset will be given from Wihan’s experimental results. We will also try to implement an interactive macro to quickly extract data from multiple locations in both aligned images.  
Challenges 
Given that the sensors are incubated with biological samples, some debris or other changes unrelated to the experimental conditions are often present in the after images. Determining accurate masks will therefore require some combination of object identification along with positional information based on surrounding objects.  Moreover, the micro-array dispensing does not always give similar images, given that there are many parameters that influence quality. Furthermore, when the sensors (such as antibodies) are fabricated/deposited at the wafer scale, there are some quality variations between the sensor spots. Lastly, since the images are taken with a laser, some diffraction patterns may appear, which will influence the measured intensities of the spots. Some of the challenges are illustrated in the following images.  

Before 

After 

C:\Users\wadi2\AppData\Local\Microsoft\Windows\INetCache\Content.MSO\65CB0FD5.tmp 

C:\Users\wadi2\AppData\Local\Microsoft\Windows\INetCache\Content.MSO\C66F65AB.tmp 

C:\Users\wadi2\AppData\Local\Microsoft\Windows\INetCache\Content.MSO\A294F7F1.tmp 

C:\Users\wadi2\AppData\Local\Microsoft\Windows\INetCache\Content.MSO\C143B027.tmp 

C:\Users\wadi2\AppData\Local\Microsoft\Windows\INetCache\Content.MSO\F29EB3CD.tmp 

C:\Users\wadi2\AppData\Local\Microsoft\Windows\INetCache\Content.MSO\F353C463.tmp 

 

# Current State of the Art 

Our approach involves first registering the two images at a wavelength chosen for sufficient outlining of the control spots, with minimal diffraction pattern from interference. Registration may require manual selection of the control spots due to the variation in imaging conditions, a GUI created using the Python library magicgui might be helpful here. Alternatively, the experimental workflow may eventually include a fiducial marker so that registration can be further automated. This automated registration will only work if the fiducial marker is contained within the image area. Next the spots of interest will need to be segmented and selected. Finally, the data extracted using the masks of interest will need to be converted into plots for analysis, and compared with the existing results that Wihan has. Each of these steps has been performed in other contexts, but utilizing them together for analysis in a hyperspectral data cube is a novel application. Therefore, the current state of the art for analyzing hyperspectral data cubes of microarrays is manual selection of areas, which we can greatly improve upon. 

 

# Performance metrics 

Our performance metrics will evolve along with the experimental design, but initially we will need some sort of manual mask created for the before and after images that can be used as a baseline for Intersect over Union comparisons after registration. We will also use F1 scores to determine the success of our automated segmentation of the spots. As many spots will be almost at the background level within the first image, we will focus on the “after” image for segmentation. The masks from that segmentation will then be transformed back onto the “before” image for quantification of the differences between the two images. 

# Proposed Timeline 

Already Completed: Manual Python analysis of user created regions of interest – includes code for plotting spectral results. 

March 1st. Create a shared GitHub repository where multiple people can work on the code. Also provides version control 

March 1st. Decide on a website 

March 8th. Determine Python functions required for affine registration of masked images 

March 8th. Determine Python functions required to generate masks of particular regions of interest 

March 15th. Enable manual annotation of regions of interest for initial alignment. 

March 22nd. Determine whether automating the detection of the control column can be consistent across a variety of test images. If not, stick with the manual annotation for the project. 

March 29th. Manual or automated creation of control masks 

March 31st. Midterm report due. 

April 10th Creation of masks for spots with high background or biological obfuscations 

April 21st. Math and output of aligned spots. 

April 26th. Create website with results/create presentation 

# Citations

1. F. Yesilkoy, E. R. Arvelo, Y. Jahani, M. Liu, A. Tittl, V. Cevher, Y. Kivshar, and H. Altug, "Ultrasensitive hyperspectral imaging and biodetection enabled by dielectric metasurfaces," Nat Photonics 13(6), 390–396 (2019). 

2. J.-H. Park, A. Ndao, W. Cai, L. Hsu, A. Kodigala, T. Lepetit, Y.-H. Lo, and B. Kanté, "Symmetry-breaking-induced plasmonic exceptional points and nanoscale sensing," Nat Phys 16(4), 462–468 (2020). 

3. D. Conteduca, I. Barth, G. Pitruzzello, C. P. Reardon, E. R. Martins, and T. F. Krauss, "Dielectric nanohole array metasurface for high-resolution near-field sensing and imaging," Nat Commun 12(1), 3293 (2021). 

4. A. Tittl, A. Leitis, M. Liu, F. Yesilkoy, D.-Y. Choi, D. N. Neshev, Y. S. Kivshar, and H. Altug, "Imaging-based molecular barcoding with pixelated dielectric metasurfaces," Science 360(6393), 1105–1109 (2018). 

5. Y. Jahani, E. R. Arvelo, F. Yesilkoy, K. Koshelev, C. Cianciaruso, M. D. Palma, Y. Kivshar, and H. Altug, "Imaging-based spectrometer-less optofluidic biosensors based on dielectric metasurfaces for detecting extracellular vesicles," Nat Commun 12(1), 3246 (2021).  
