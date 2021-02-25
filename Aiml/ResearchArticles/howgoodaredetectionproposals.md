# How good are detection proposals, really?

## Abstract

Object detectors employ detection proposals to guide the search for objects thereby avoiding exhaustive sliding window search across images. However, it is unclear which trade-offs are made when using them during object detection.

## Introduction

Detection proposals allow to overcome the issue between computational tractability(with sliding window detectors requiring $10^6$ classifier evaluations per image) and high detection quality. 

It works under the assumption that all objects of interest share common visual properties that distinguish them from the background. Under this assumption, a method can be trained that, given an input image, outputs a set of detection window proposals that are likely to contain objects. Achieving high accuracies with $10^4$ or less windows then **significant speeds-ups** can be achieved. Apart from improving speed, detection proposals help to **improve the detection quality** by reducing false positives.

## Detection proposal methods

Detection proposal methods are based on low-level image features to generate candidate windows. Given the low-level features, the method quickly decides whether a window should be considered for detection or not.
