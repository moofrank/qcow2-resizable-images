qcow2-resizable-images
======================

Kickstart and OZ template file to build a qcow2 compressed, sparsed and resizable and first boot

INSTRUCTIONS:

1. Do not reduce the disk size in the OZ's  Template file (<size>3G</size>), otherwise the automatica install will fail.

2. To build the raw image execute the command:

	oz-install -d 4 -t 7200 -u -a [kickstart-file] -s /< wherever-you-can-write >/centos65_x86_64.dsk oz.tdl

3. Turn the built image into QCOW2 compressed format:

	qemu-img convert -c -O qcow2 /< wherever-you-can-write >/centos65_x86_64.dsk /< wherever-you-can-write >/centos65_x86_64.qcow2

4 (Optional) Sparsify tthe image (you can same some more space):

	virt-sparsify --compress /< wherever-you-can-write >/centos65_x86_64.qcow2 /< wherever-you-can-write >/centos65_x86_64-sparse.qcow2

5 Load the QCOW2 image into Glance, taking care of the correct minimum disk space specification::

	glance image-create --progress --name="CentOS 6.5 - x64" --disk-format=qcow2 --container-format=bare --is-public=False --min-disk 3 --min-ram 512 < /< wherever-you-can-write >/centos65_x86_64-sparse.qcow2

Note: after step 4 check that the sparsed file's size is actually smaller than the qcow2. In fact, in some cases this step can increase it depending on how you filled the root disk.
