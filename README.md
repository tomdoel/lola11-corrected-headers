This repository is to assit researchers who want to load LOLA11 challenge datasets into MetaIO compatible applications such as PTK or ITK-Snap.

The LOLA data can be obtained from the [LOLA11 grand challenge website](https://lola11.grand-challenge.org) after registration.

Note this repository is not affiliated in any way with the LOLA11 challenge. 

Image data are not provided here. Copyright of image data remains with the original providers.

Please respect the rules of the challenge and data copyright when downloading and using the data.

The following is based on my understanding of the MetaIO standard. I do not guarantee accuracy so use this information at your own risk.


## Purpose

The LOLA11 datasets are provided in METAIO (`.mhd/.raw`) format.

The `TransformMatrix` header fields for some of these datasets are not set correctly for loading into applications such as PTK or ITK-Snap.

This repository provides replacement `.mhd` files with the `TransformMatrix` and `AnatomicalOrientation` fields corrected. 

Note that you must obtain the image data (`.raw` files) from the [LOLA11 grand challenge website](https://lola11.grand-challenge.org).

## Notes about `TransformMatrix` and `AnatomicalOrientation`

See [ITK MetaIO documentation](https://itk.org/Wiki/Proposals:Orientation#Some_notes_on_the_DICOM_convention_and_current_ITK_usage).

`TransformMatrix` defines direction cosines for the image, which are essentially the same as those defined in the Dicom standard. However, in Dicom you are generally dealing with slices which only need to specify direction cosines for two directions of an image slice; the third direction cosine is not required since each image slice has its own position tag. In MetaIO volumes there needs to be a third direction cosine to identiy the direction of the third axis in the volume.

`AnatomicalOrientation` is not required if `TransformMatrix` is set, but if defined it should be consistent. Note that Meta IO use the opposite conventions to Dicom, so `L<->R`, `A<->P`, `I<->S` when converting between the two.

For example, a `TransformMatrix` of `1 0 0 0 1 0 0 0 1` corresponds to `RAI`, while a `TransformMatrix` of `1 0 0 0 1 0 0 0 -1` corresponds to `RAS`

In general, the LOLA datasets with  `AnatomicalOrientation` set to `RAI` have correct `AnatomicalOrientation` and `TransformMatrix`, while those with `RPI` should actually be `RAS` and have a `TransformMatrix` of `1 0 0 0 1 0 0 0 -1`.

In addition, dataset 9 has the correct `TransformMatrix` but the `AnatomicalOrientation` should be `RAS`.

Note: PTK and ITK-Snap will ignore the `AnatomicalOrientation` field if the `TransformMatrix` field is set; however it should always be consistent. However, the `AnatomicalOrientation` field ought to be be consistent with the `TransformMatrix` field).


