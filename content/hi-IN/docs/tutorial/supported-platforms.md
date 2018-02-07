# समर्थित प्लेटफ़ॉर्म

निम्नलिखित प्लेटफार्म इलेक्ट्रॉन द्वारा समर्थित हैं:

### मैकओएस

मैकओएस के लिए केवल 64-बिट बाइनरिस प्रदान की गयी हैं, और न्यूनतम मैकओएस समर्थित संस्करण मैकओएस 10.9 है |

### विंडोज

विंडोज 7 और उसके बाद के ओएस समर्थित हैं | पुराने ओएस समर्थित नहीं हैं (न ही वे काम करते हैं) |

विंडोज के लिए दोनों `ia32` (`x86`) और `x64` (`amd64`) बाइनरिस प्रदान की गयी हैं |. कृपया ध्यान दें, विंडोज का `ARM` संस्करण अभी समर्थित नहीं है |

### लिनक्स

The prebuilt `ia32` (`i686`) and `x64` (`amd64`) binaries of Electron are built on Ubuntu 12.04, the `armv7l` binary is built against ARM v7 with hard-float ABI and NEON for Debian Wheezy.

[Until the release of Electron 2.0](https://github.com/electron/electron/blob/master/docs/tutorial/planned-breaking-changes.md#duplicate-arm-assets), Electron will also continue to release the `armv7l` binary with a simple `arm` suffix. Both binaries are identical.

Whether the prebuilt binary can run on a distribution depends on whether the distribution includes the libraries that Electron is linked to on the building platform, so only Ubuntu 12.04 is guaranteed to work, but following platforms are also verified to be able to run the prebuilt binaries of Electron:

* उबुन्तु 12.04 और उसके बाद के संस्करण
* फेडोरा 21
* डेबियन 8