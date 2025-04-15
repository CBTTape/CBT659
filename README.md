# CBT659
Converted to GitHub via [cbt2git](https://github.com/wizardofzos/cbt2git)

This is still a work in progress. 
Due to amazing work by Alison Zhang and Jake Choi repos are no longer deleted.

```
//***FILE 659 is from Glenn Siegel and contains a recipe for        *   FILE 659
//*           creating a pc-based DASD volume from a mainframe      *   FILE 659
//*           DASD volume.                                          *   FILE 659
//*                                                                 *   FILE 659
//*           It is recommended to use HERC306 and WINPCAP          *   FILE 659
//*             members, and to NOT USE member HERC216.             *   FILE 659
//*                                                                 *   FILE 659
//*           Members H312W** (32 or 64) contain a later            *   FILE 659
//*             version of Hercules, but do not include             *   FILE 659
//*             the CTC-WIN component, which should be              *   FILE 659
//*             either purchased from David Trout at                *   FILE 659
//*                     www.softdevlabs.com                         *   FILE 659
//*             or extracted, as separate files, from the           *   FILE 659
//*             directory resulting from unzipping member           *   FILE 659
//*             HERC306.                                            *   FILE 659
//*                                                                 *   FILE 659
//*     The purpose of this file is to enable the user to create    *   FILE 659
//*      a CKD_P370 PC DASD emulated file from a mainframe DASD     *   FILE 659
//*      volume, for use on the following mainframe emulators       *   FILE 659
//*      P/390, Flex-ES and Hercules.  It is your responsibility    *   FILE 659
//*      to be licensed for any products discussed in these         *   FILE 659
//*      documents.                                                 *   FILE 659
//*                                                                 *   FILE 659
//*     Included in this file are the following freeware            *   FILE 659
//*      software programs GZIP for the PC Version 1.2.4, GZIP      *   FILE 659
//*      for the Mainframe Version 1.2.3, AWSTAPE source code,      *   FILE 659
//*      XmitManager Version 3, Hercules Version 2.16.5 and all     *   FILE 659
//*      the necessary CYGWIN dll's.                                *   FILE 659
//*                                                                 *   FILE 659
//*     Hercules 3.06 with Fish's DLLs are included, zipped, in     *   FILE 659
//*      the HERC306 member.  WINPCAP 4.0.2 is included as an       *   FILE 659
//*      *.exe file, in member WINPCAP.  Use these, rather than     *   FILE 659
//*      member HERC216, which is old.                              *   FILE 659
//*                                                                 *   FILE 659
//*     Again, if you want to install Hercules 3.12, it will        *   FILE 659
//*      not include Fish's DLLs.  These should be copied from      *   FILE 659
//*      an unzip of member HERC306.  (You'll have to figure        *   FILE 659
//*      out which are the extra files which belong to Fish's       *   FILE 659
//*      DLLs.  Or buy CTC-WIN from www.softdevlabs to make         *   FILE 659
//*      the process easier and more straightforward.               *   FILE 659
//*                                                                 *   FILE 659
//*     --------------------------------------------------------    *   FILE 659
//*                                                                 *   FILE 659
//*     Hercules 3.08 64-bit without Fish's DLL's are included,     *   FILE 659
//*      zipped, in the HERC308W member.  Fish's new DLL's were     *   FILE 659
//*      very large, and are only needed if you want to set up      *   FILE 659
//*      FTP from your PC to your Hercules system.  So I was        *   FILE 659
//*      forced to omit them for lack of space.  This is a zip      *   FILE 659
//*      of the binary Hercules directory that you can run on       *   FILE 659
//*      the PC.  Just download this member in BINARY to a          *   FILE 659
//*      directory on the PC, and type (in a DOS shell, pointing    *   FILE 659
//*      to that directory (cd \directory)                          *   FILE 659
//*                                                                 *   FILE 659
//*      hercules -f <fully.pathed.config.file>                     *   FILE 659
//*                                                                 *   FILE 659
//*     To see the full version of Hercules 3.08, with Fish's       *   FILE 659
//*      (David Trout's) DLL's installed, see CBT File 889.         *   FILE 659
//*                                                                 *   FILE 659
//*     --------------------------------------------------------    *   FILE 659
//*                                                                 *   FILE 659
//*     I've packaged this so that you can use GZIP or not.         *   FILE 659
//*                                                                 *   FILE 659
//*     GZIP    - Depending upon the size of the volume your        *   FILE 659
//*               going to create and your CPU size you may want    *   FILE 659
//*               to use GZIP.  The mainframe version of GZIP is    *   FILE 659
//*               very CPU intensive.  The PC version is very       *   FILE 659
//*               fast.                                             *   FILE 659
//*                                                                 *   FILE 659
//*     No GZIP - The file can be rather large depending upon       *   FILE 659
//*               the file type and density of the DASD volume      *   FILE 659
//*               being backed up.  The file size you can expect    *   FILE 659
//*               for a 3390-3 can be up to and over 2 Gig, the     *   FILE 659
//*               ratio is about 1.25 cylinders per meg.            *   FILE 659
//*                                                                 *   FILE 659
//*     Members included                                            *   FILE 659
//*                                                                 *   FILE 659
//*      $$README     This document.                                *   FILE 659
//*      $INSTRUC     Instructions for creating a CKD_P370 PC       *   FILE 659
//*                   file.                                         *   FILE 659
//*      ALLOC        JCL to allocate a large dataset.  Needed to   *   FILE 659
//*                   FTP XMITs of large volume backups from the    *   FILE 659
//*                   PC to the mainframe, etc. etc.                *   FILE 659
//*      AWSTAPE      Program AWSTAPE and JCL to create a           *   FILE 659
//*                   AWSTAPE DASD file.                            *   FILE 659
//*      DASDINIT     Help instructions for using the Hercules      *   FILE 659
//*                   dasdinit program on the PC to create new      *   FILE 659
//*                   DASD volumes there.                           *   FILE 659
//*      DFDSSBK      JCL to create a DFDSS backup DASD file.       *   FILE 659
//*      DFDSSSAR     JCL to create a DFDSS stand alone restore     *   FILE 659
//*                   card DASD file.                               *   FILE 659
//*      DFDSSRS      JCL to restore DFDSS backup DASD file         *   FILE 659
//*                   onto an initialized target disk pack.         *   FILE 659
//*      DISCLAIM     Legal stuff.                                  *   FILE 659
//*      DISKMAP      Handy mapping program that tells you the      *   FILE 659
//*                   original size of DASD, so you can make an     *   FILE 659
//*                   appropriately small or large PC DASD (using   *   FILE 659
//*                   the Hercules dasdinit program to restore)     *   FILE 659
//*                   to restore the pack onto.  This is JCL.       *   FILE 659
//*                   The program is in LOADLIB.                    *   FILE 659
//*      FDRBACKV     FDR full volume DASD backup if you don't      *   FILE 659
//*                   want to use DFDSS.                            *   FILE 659
//*      FDRRESTV     FDR full volume restore JCL for the FDR       *   FILE 659
//*                   full volume DASD backup.                      *   FILE 659
//*      GZIP         GZIP Version 1.2.4 PC exe file.               *   FILE 659
//*      GZIPFILE     JCL to create a GZIP DASD file.               *   FILE 659
//*      GZIPXMIT     XMIT format mainframe GZIP Version 1.2.3      *   FILE 659
//*                   loadlib.                                      *   FILE 659
//*      HERC216      Hercules Version 2.16.5 PC zipped file.       *   FILE 659
//*      HERC306      Hercules Version 3.06 MSVC - zipped file.     *   FILE 659
//*      HERC308W     Hercules Version 3.08 MSVC - zipped file.     *   FILE 659
//*                    (Fish's DLL's are not included in this       *   FILE 659
//*                    member.)                                     *   FILE 659
//*      INITPACK     INIT a target DASD pack (offline) to          *   FILE 659
//*                   restore the old pack onto, using the          *   FILE 659
//*                   DFDSSRS job (after the pack is online)        *   FILE 659
//*      LOADLIB      XMIT-format file containing ZAP and DISKMAP.  *   FILE 659
//*      QUICKINS     Quick instructions for the brave.             *   FILE 659
//*      WINPCAP      Win-Pcap 4_0_2 installation file. *.exe       *   FILE 659
//*      XMITFILE     JCL to create a XMIT DASD file.               *   FILE 659
//*      XMITMAN      XmitManager Version 3 PC install zipped       *   FILE 659
//*                   file.                                         *   FILE 659
//*                                                                 *   FILE 659
//*   Sometimes when you FTP a large XMIT file, its DCB attributes  *   FILE 659
//*   get messed up.  The ZAP program is provided to fix these,     *   FILE 659
//*   if there is no other way to do it.  You can also use the      *   FILE 659
//*   CDSCB TSO command from CBT Tape File 300 for that purpose.    *   FILE 659
//*                                                                 *   FILE 659
//*     Author: Glenn Siegel           Updated April 2009           *   FILE 659
//*             S.S.C. Corp.           by SBG                       *   FILE 659
//*             Glenn54@aol.com                                     *   FILE 659
//*             631-444-5339                                        *   FILE 659
//*             516-607-4005 Cell                                   *   FILE 659
//*                                                                 *   FILE 659
```
