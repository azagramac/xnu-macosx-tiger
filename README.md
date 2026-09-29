# How to Build XNU Kernel
<img height="350" alt="osx-logo" src="https://github.com/user-attachments/assets/2b375aee-5c57-4456-9460-32c63890bea9" />

## 1. Building XNU

Type:

```sh
make
```

This builds all the components for all architectures defined in `ARCH_CONFIGS` and for all kernel configurations defined in `KERNEL_CONFIGS`.

By default:

* `ARCH_CONFIGS` contains one architecture, the build machine architecture.
* `KERNEL_CONFIGS` is set to build for `RELEASE`.

This will also create a bootable image, `mach_kernel`, and a kernel binary with symbols, `mach_kernel.sys`.

### Example

```text
$(OBJROOT)/RELEASE_PPC/osfmk/RELEASE/osfmk.o: pre-linked object for osfmk component
$(OBJROOT)/RELEASE_PPC/mach_kernel: bootable image
```

---

## 2. Building a Component

Go to the top directory in your XNU project.

If you are using a **sh-style shell**, run:

```sh
. SETUP/setup.sh
```

If you are using a **csh-style shell**, run:

```csh
source SETUP/setup.csh
```

This will define the following environment variables:

```text
SRCROOT
OBJROOT
DSTROOT
SYMROOT
```

### Build the component

From a component top directory:

```sh
make all
```

This builds a component for all architectures defined in `ARCH_CONFIGS` and for all kernel configurations defined in `KERNEL_CONFIGS`.

By default:

* `ARCH_CONFIGS` contains one architecture, the build machine architecture.
* `KERNEL_CONFIGS` is set to build for `RELEASE`.

### Example

```text
$(OBJROOT)/RELEASE_PPC/osfmk/RELEASE/osfmk.o: pre-linked object for osfmk component
```

### Build `mach_kernel`

From the component top directory:

```sh
make mach_kernel
```

This includes your component in the bootable image, `mach_kernel`, and in the kernel binary with symbols, `mach_kernel.sys`.

> **WARNING:** If a component header file has been modified, you will have to perform the complete build procedure described in **Section 1**.

---

## 3. Building DEBUG

Define `KERNEL_CONFIGS` to `DEBUG` in your environment or when running a `make` command.

Then apply the appropriate build procedures.

### Using `make`

```sh
make KERNEL_CONFIGS=DEBUG all
```

### Using an environment variable

```sh
export KERNEL_CONFIGS=DEBUG
make all
```

### Example

```text
$(OBJROOT)/DEBUG_PPC/osfmk/DEBUG/osfmk.o: pre-linked object for osfmk component
$(OBJROOT)/DEBUG_PPC/mach_kernel: bootable image
```

---

## 4. Building Fat

Define `ARCH_CONFIGS` in your environment or when running a `make` command.

### Using `make`

```sh
make ARCH_CONFIGS="PPC I386" exporthdrs all
```

### Using an environment variable

```sh
export ARCH_CONFIGS="PPC I386"
make exporthdrs all
```

---

## 5. Build Check Before Integration

From the top directory, run:

```sh
~rc/bin/buildit . -arch ppc -arch i386 -noinstallsrc -nosum
```

---

## 6. Creating Tags and Cscope

Set up your build environment according to the instructions in **Section 2**.

From the top directory, run:

### Create tags

```sh
make tags
```

This will build `ctags` and `etags`.

### Create the cscope database

```sh
make cscope
```

This will build the `cscope` database.

