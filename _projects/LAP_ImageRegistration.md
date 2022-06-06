---
layout: page
title: Multi-Dimensional Signal Alignment
description: Local All-Pass Filter Framework
img: assets/img/LAP_Project/LAP_Diagram.png
importance: 1
category: work
---
The estimation of a geometric transformation that aligns two or more signals is problem that has many applications in signal processing. The problem occurs when signals are either recorded from two or more spatially separated sensors or when a single sensor is recording a time-varying scene. Examples of fundamental tasks that involve this problem are shown in the figure below. In this project we estimate the transformation between these signals using a novel **local all-pass (LAP) filtering framework**.

<div class="row">
    <div class="col-sm-1 mt-0">
    </div>
    <div class="col-sm-10 mt-0">
        {% include figure.html path="assets/img/LAP_Project/Signal_Alignment_Problems.png" title="Applications of Signal Alignment" class="img-fluid rounded z-depth-1" caption="Applications requiring multi-dimensional signal alignment." zoomable=true %}
    </div>
    <div class="col-sm-1 mt-0">
    </div>
</div>

The underlying principle in our LAP framework is that, on a local level, the geometric transformation between a pair of signals can be approximated as a rigid deformation which is equivalent to an all-pass filtering operation. Thus, efficient estimation of the all-pass filter in question allows an accurate estimation of the local geometric transformation between the signals. Accordingly, repeating this estimation for every sample/pixel/voxel in the signals results in a dense, estimation of the whole geometric transformation. This processing chain can be performed efficiently and achieve very accurate results. We have applied this framework to image registration {% cite Gilliam2018 Gilliam2015 Zhang2020 %}, motion correction {% cite Kustner2017 Gilliam2016a %} and time-varying delay estimation {% cite Gilliam2018b %}.

<br>

<div class="accordion" id="accordionExample">
  <div class="card">
    <!-- Start: The Problem description -->
    <div class="card-header" id="headingOne">
      <h2 class="mb-0">
        <button class="btn btn-link btn-block text-left" type="button" data-toggle="collapse" data-target="#collapseOne" aria-expanded="true" aria-controls="collapseOne">
        The Problem
        </button>
      </h2>
    </div>
    <div id="collapseOne" class="collapse" aria-labelledby="headingOne" data-parent="#accordionExample">
      <div class="card-body" id="The_Problem">
        Our problem comprises finding a geometric transform \(\mathcal{T}\) between two signals based on the variation of their sample intensities. We consider a non-rigid transformation characterized by a sample-wise deformation field \(\mathrm{u}\) of the form:

        $$ \mathcal{T}({\bf x}) = {\bf x} + {\bf u}({\bf x}), $$

        where \(\mathbf{x} = [x_1,x_2,\ldots,x_n]^{T}\) is the nth dimensional sample coordinate and \(\mathbf{u}(\mathbf{x}) = [u_1(\mathbf{x}),u_2(\mathbf{x}),\ldots,u_n(\mathbf{x})]^{T}\) is the vector field representing the deformation. We formulate the estimation of this transform assuming the brightness consistency hypothesis: a sample's intensity remains constant under the deformation. Thus, given two signals \(I_1(\mathbf{x})\) and \(I_2(\mathbf{x})\), our problem is to find a deformation field that relates these signals as follows:

        $$ I_2(\mathbf{x}+\mathbf{u}(\mathbf{x})) = I_1(\mathbf{x}).$$

        <blockquote> <em><strong>Remark:</strong> This problem is both restrictive, as the brightness consistency is unlikely to be satisfied exactly, and ill-posed as, for \(n>1\) many deformations many satisfy the equation and most are meaningless. However, in many applications, it is important to determine a meaningful deformation field.</em> </blockquote>

        To overcome these challenges, we assume the deformation field is slowly varying such that locally it is equivalent to a rigid deformation and apply our local all-pass filter framework.

      </div>
    </div>
    <!-- End: The Problem Description -->
  </div>

  <div class="card">
    <!-- Start: LAP Framework -->
    <div class="card-header" id="headingTwo">
      <h2 class="mb-0">
        <button class="btn btn-link btn-block text-left collapsed" type="button" data-toggle="collapse" data-target="#collapseTwo" aria-expanded="false" aria-controls="collapseTwo">
          Local All-Pass Filter Framework
        </button>
      </h2>
    </div>
    <div id="collapseTwo" class="collapse" aria-labelledby="headingTwo" data-parent="#accordionExample">
      <div class="card-body">
        The central concept in our framework that a rigid deformation is equivalent to filtering with an all-pass filter. This filter can be estimated efficiently by leveraging the unique frequency structure of an all-pass filter to obtain a linear forward-backward filtering relation as shown in the figure below. Importantly, as the forward-backward filtering is linear in \(p\), it is straightforward and efficient to solve, see {% cite Gilliam2018 Blu2015 %} for more details.<br>
        <br>
        <div class="row">
          <div class="col-sm-7 mx-auto">
            {% include figure.html path="assets/img/LAP_Project/AP_Fig2.png" title="All-Pass Filtering Relationship" class="img-fluid rounded z-depth-1" caption="Forward-Backward Filtering Relationship" zoomable=true %}
          </div>
        </div>
        <br>
        To allow for a non-rigid deformation, we limit this estimation to a small local region, estimate a local all-pass filter and extract a local estimate of the deformation. This local estimate corresponds to the centre of the region. Accordingly, a dense, per-sample, deformation estimate is obtained by repeating this process for all the samples in the signal using a sliding-window mechanism, see figure below. Importantly, as discussed in {% cite Gilliam2018 %} this can be performed very efficiently. <br>

        <br>
        <div class="row">
          <div class="col-sm-7 mx-auto">
            {% include figure.html path="assets/img/LAP_Project/LAP_Diagram.png" title="LAP Algorithm" class="img-fluid rounded z-depth-1" caption="Illustration of the Local All-Pass Filter Framework" zoomable=true %}
          </div>
        </div>

      </div>
    </div>
    <!-- End: LAP Framework -->
  </div>

  <div class="card">
    <!-- Start: Image Registration -->
    <div class="card-header" id="headingThree">
      <h2 class="mb-0">
        <button class="btn btn-link btn-block text-left collapsed" type="button" data-toggle="collapse" data-target="#collapseThree" aria-expanded="false" aria-controls="collapseThree">
          Image Registration
        </button>
      </h2>
    </div>
    <div id="collapseThree" class="collapse" aria-labelledby="headingThree" data-parent="#accordionExample">
      <div class="card-body">
        We have applied our LAP framework to both non-rigid {% cite Gilliam2018 %} and parametric {% cite Zhang2020 %} image registration. An example of the results obtained from the LAP for a non-rigid registration is shown in the figure below. The input images are shown in (a) and (c), ground truth deformation in (b) and the LAP estimated deformation in (d). Note that each colour represents the direction of the deformation and a colour code is shown in (e).<br>
        <br>
        <div class="row">
          <div class="col-sm-4">
            {% include figure.html path="assets/img/LAP_Project/Image1_LAP_2D.png" title="Image 1" class="img-fluid rounded z-depth-1" caption="(a) Image 1" zoomable=true %}
          </div>
          <div class="col-sm-4">
            {% include figure.html path="assets/img/LAP_Project/GTFlow_LAP_2D.png" title="Ground Truth Deformation" class="img-fluid rounded z-depth-1" caption="(b) True Deformation" zoomable=true %}
          </div>
          <div class="col-sm-4">
            {% include figure.html path="assets/img/LAP_Project/Image2_LAP_2D.png" title="Image 2" class="img-fluid rounded z-depth-1" caption="(c) Image 2" zoomable=true %}
          </div>
        </div>
        <div class="row">
          <div class="col-sm-2">
          </div>
          <div class="col-sm-4">
            {% include figure.html path="assets/img/LAP_Project/LAP_Flow_LAP_2D.png" title="LAP Deformation Estimate" class="img-fluid rounded z-depth-1" caption="(d) LAP Deformation" zoomable=true %}
          </div>
          <div class="col-sm-4">
            {% include figure.html path="assets/img/LAP_Project/Colour_Wheel.png" title="Deformation Colour Code" class="img-fluid rounded z-depth-1" caption="(e) Deformation Colour Code" zoomable=true %}
          </div>
        </div>
        <div class="caption">
          Illustration of the smoothly varying deformation and its estimation using the LAP framework. The first image is shown in (a), the smoothly varying deformation in (b), the second image in (c) and the LAP deformation estimate in (d). The colour coding for the deformation (each colour represents a different direction of the deformation) is in (e). Note that the deformation has a maximum displacement of 15 pixels. {% cite Gilliam2015 %}
        </div>
        <br>
        To allow estimation of both slowly and quickly varying deformations, we use an iterative poly-filter LAP framework that starts with large filters estimating the deformation, aligning the images and then repeating with a smaller filter {% cite Gilliam2018 %}.<br>
        <br>
        <div class="row">
          <div class="col-sm-3">
          </div>
          <div class="col-sm-6">
            {% include figure.html path="assets/img/LAP_Project/MS_LAP_2D.png" title="Poly-Filter LAP" class="img-fluid rounded z-depth-1" caption="Poly-Filter LAP Framework" zoomable=true %}
          </div>
        </div>
        <br>
        <h5> Parametric Registration</h5>
        For parametric image registration, we introduce a quadratic parametric model for the deformation and iteratively estimate the parameters of the model {% cite Zhang2020 Zhang2017 Zhang2019c %}. This parametric extension is robust to model mis-match (noise, blurring, etc), very accurate and capable of handling very large deformations. Furthermore, by modeling intensity variations, the parametric LAP is capable of handling multi-modal registration problems.<br>
        <br>
        <div class="row">
          <div class="col-sm-4">
            {% include figure.html path="assets/img/LAP_Project/Image1_Para_LAP_2D.jpg" title="Image 1" class="img-fluid rounded z-depth-1" caption="(a) Image 1" zoomable=true %}
          </div>
          <div class="col-sm-4">
            {% include figure.html path="assets/img/LAP_Project/Image2_Para_LAP_2D.jpg" title="Image 2" class="img-fluid rounded z-depth-1" caption="(b) Image 2" zoomable=true %}
          </div>
          <div class="col-sm-4">
            {% include figure.html path="assets/img/LAP_Project/Result_Para_LAP_2D.jpg" title="Parametric LAP Registration" class="img-fluid rounded z-depth-1" caption="(c) LAP Registration" zoomable=true %}
          </div>
        </div>
        <div class="caption">
          Illustration of the results of applying the parametric LAP. The first image is shown in (a), the second image in (b) and the registration of image 2 to image 1 using the deformation obtained from the parametric LAP. {% cite Zhang2020 %}
        </div>
      </div>
    </div>
    <!-- End: Image Registration -->
  </div>

  <div class="card">
    <!-- Start: MRI Motion Correction -->
    <div class="card-header" id="headingFour">
      <h2 class="mb-0">
        <button class="btn btn-link btn-block text-left collapsed" type="button" data-toggle="collapse" data-target="#collapseFour" aria-expanded="false" aria-controls="collapseFour">
          Motion Correction for MR Images
        </button>
      </h2>
    </div>
    <div id="collapseFour" class="collapse" aria-labelledby="headingFour" data-parent="#accordionExample">
      <div class="card-body">
        We have applied our LAP framework to 3D volumetric magnetic resonance imaging (MRI) data to remove artefacts caused by respiratory motion {% cite Gilliam2016 %}. We first estimate the deformation field between the two images (i.e. determining the respiratory motion) and then remove the motion by registering the moving image to the fixed image. Our approach outperformed existing 3D non-rigid registration algorithms in both accuracy and computation speed and was incorporate into a joint MRI/PET motion correction system {% cite Kustner2017 %}. An example of the results obtained from the 3D LAP for respiratory motion estimation on an <em>in-vivo</em> MRI dataset is shown below.<br>
        <br>
        <div class="row">
          <div class="col-sm-4">
            {% include figure.html path="assets/img/LAP_Project/Input_Images_MRI_P3.gif" title="Fixed-Moving MRI data" class="img-fluid rounded z-depth-1" caption="(a) Fixed and moving MRI data" zoomable=true %}
          </div>
          <div class="col-sm-4">
            {% include figure.html path="assets/img/LAP_Project/Flow_LAP_MRI_P3.png" title="LAP Deformation" class="img-fluid rounded z-depth-1" caption="(b) LAP deformation" zoomable=true %}
          </div>
          <div class="col-sm-4">
            {% include figure.html path="assets/img/LAP_Project/LAP_MRI_P3.gif" title="Fixed-Registered MRI data" class="img-fluid rounded z-depth-1" caption="(c) Fixed and LAP registered MRI data" zoomable=true %}
          </div>
        </div>
        <div class="caption">
          Illustration of the motion correction results obtained using the 3D LAP. Part (a) shows an animation of a 2D coronial slice of the fixed and moving images with motion, part (b) shows a 2D slice of the 3D deformation estimated by the LAP, and part (c) shows an animation of a 2D coronial slice of the fixed and LAP registered images with motion removed.{% cite Gilliam2016 %}
        </div>
        <br>

        To allow estimation of both slowly and quickly varying deformations, we use an iterative poly-filter 3D LAP framework that starts with large filters estimating the deformation, aligning the images and then repeating with a smaller filter {% cite Gilliam2016 %}.<br>
        <br>
        <div class="row">
          <div class="col-sm-6 mx-auto">
            {% include figure.html path="assets/img/LAP_Project/MS_LAP_3D.png" title="Poly-filter 3D LAP" class="img-fluid rounded z-depth-1" caption="Poly-Filter 3D LAP Framework" zoomable=true %}
          </div>
        </div>
        <br>
        <h5>LAP + Deep Learning</h5>
        More recently, the LAP framework has been combined with a deep learning architecture to allow robust motion correction when faced with highly-accelerated, undersampled, MRI data {% cite Kustner2021 %}. This network has also been combined with a reconstruction network to allow motion-corrected reconstruction of 4D (3D + time) MRI data {% cite Kustner2020 Kustner2022 %}. The architecture of this LAP deep learning network (LAPNet) is shown below.
        <br>
        <br>
        <div class="row">
          <div class="col-sm-11 mx-auto">
            {% include figure.html path="assets/img/LAP_Project/Motion_DL_Network.png" title="LAPNet architecture" class="img-fluid rounded z-depth-1" zoomable=true %}
          </div>
        </div>
        <div class="caption">
          Proposed LAPNet architecture to perform non-rigid registration in MRI k-space. {% cite Kustner2021 %}
        </div>
        <br>

      </div>
    </div>
    <!-- End: MRI Motion Correction -->
  </div>

  <div class="card">
    <!-- Start: Time-Varying Delay Estimation -->
    <div class="card-header" id="headingFive">
      <h2 class="mb-0">
        <button class="btn btn-link btn-block text-left collapsed" type="button" data-toggle="collapse" data-target="#collapseFive" aria-expanded="false" aria-controls="collapseFive">
          Time-Varying Delay Estimation
        </button>
      </h2>
    </div>
    <div id="collapseFive" class="collapse" aria-labelledby="headingFive" data-parent="#accordionExample">
      <div class="card-body">
        We have applied our LAP framework to 1D signals recorded from spatial separate sensors to estimate a 1D deformation, which is normally referred to as a time-varying delay signal {% cite Gilliam2018b %}. Furthermore, we extend our framework to allow for the estimation of a deformation that is common to an ensemble of signals, which we term the Common LAP (CLAP). Illustrations of the 1D LAP framework and the 1D CLAP framework are shown below.<br>
        <br>
        <div class="row">
          <div class="col-sm-6">
            {% include figure.html path="assets/img/LAP_Project/LAP_Fig_1D.png" title="1D LAP Framework" class="img-fluid rounded z-depth-1" caption="(a) 1D LAP Framework" zoomable=true %}
          </div>
          <div class="col-sm-6">
            {% include figure.html path="assets/img/LAP_Project/CLAP_Diagram.png" title="1D CLAP Framework" class="img-fluid rounded z-depth-1" caption="(b) 1D CLAP Framework" zoomable=true %}
          </div>
        </div>
        <div class="caption">
          Illustration of the LAP framework for a pair of 1D signals in (a) and the CLAP framework for an ensemble of 1D signals in (b). The CLAP framework estimates a set of local all-pass filters that are common to the ensemble of signals.
        </div>
        To allow estimation of both slowly and quickly varying deformations, we use an iterative multi-scale CLAP framework that starts with large filters estimating the deformation, aligning the images and then repeating with a smaller filter {% cite Gilliam2018b %}.<br>
        <br>
        <div class="row">
          <div class="col-sm-6 mx-auto">
            {% include figure.html path="assets/img/LAP_Project/CLAP_MS.png" title="Multi-scale 1D CLAP Framework" class="img-fluid rounded z-depth-1" caption="Multi-scale 1D CLAP Framework" zoomable=true %}
          </div>
        </div>
        <br>
        <br>
        <h5>High-Density Surface EMG (HD-sEMG):</h5>
        We use our CLAP framework to estimate conduction velocity (CV) from high-density surface electromyography (sEMG) recordings {% cite Gilliam2018b Gilliam2018d %}. CV describes the speed of propagation of motor unit action potentials (MUAPs) along the muscle fibre and is an important factor in the study of muscle activity revealing information regarding pathology, fatigue or pain in the muscle.<br>
        <br>
        <div class="row">
          <div class="col-sm-5">
            {% include figure.html path="assets/img/LAP_Project/EMG_Recording.png" title="1D CLAP Framework" class="img-fluid rounded z-depth-1" caption="(a) HD-sEMG Data Acquisition" zoomable=true %}
          </div>
          <div class="col-sm-7">
            {% include figure.html path="assets/img/LAP_Project/Real_HD_sEMG_data.png" title="1D CLAP Framework" class="img-fluid rounded z-depth-1" caption="(b) HD-sEMG Data" zoomable=true %}
          </div>
        </div>
        <div class="caption">
          Illustration of acquiring high-density sEMG data from the bicep, (a), and the corresponding ensemble of electrode signals with a common time-varying delay, (b).
        </div>
      </div>
    </div>
    <!-- End: Time-Varying Delay Estimation -->
  </div>

  <div class="card">
    <!-- Start: Code -->
    <div class="card-header" id="headingSix">
      <h2 class="mb-0">
        <button class="btn btn-link btn-block text-left collapsed" type="button" data-toggle="collapse" data-target="#collapseSix" aria-expanded="false" aria-controls="collapseSix">
          Code
        </button>
      </h2>
    </div>
    <div id="collapseSix" class="collapse" aria-labelledby="headingSix" data-parent="#accordionExample">
      <div class="card-body">

        Motion Correction for MRI code can be found: <a href="https://github.com/thomaskuestner/MoCoGUI" target="_blank">here</a> <br>
        <br>
        Delay estimation code can be found: <a href="https://github.com/beteje/LAP_DelayEstimation" target="_blank">here</a><br>
        This code contains the several implementations of the LAP framework and a signal generation function which has a number of different delay functions.
      </div>
    </div>
    <!-- End: Code -->
  </div>

</div>

<br>
---
## References
<div class="references">
  {% bibliography --cited_in_order --group_by none %}
</div>
