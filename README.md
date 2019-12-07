# Multi-step build container template for Node.js applications

This is a simple multi-step Node.js application docker build template that you can use to create self-contained, relatively small-sized and easily deployable images of your application.

## Why multi-step

There are several benefits to this, but one of the bigger ones I would say is that your actual eventual container will not have any of the build tools installed, which can be safer or at the very least cleaner. Your application does not require yarn to run correctly, after all, only your build (and test) steps need this. So by splitting it out into multiple, stand-alone steps, your final image can be safer, and smaller too. You could even go one step further and flatten the built image to reduce the size even further, but that is outside the scope of this readme.

## Node versions

- The template uses `node:13`, but be sure to update this to whatever specific version of node your application requires. Make sure to set this accordingly in all three build steps, too.
- The final stage (which actually runs your application) uses the `node:slim` image, a lighter version that only contains whatever is required to actually run Node. If your application or specific packages used is having issues running in this environment, consider changing the final phase to use `node:alpine` instead. Using the full image here would mean it still will come with tools like NPM and Yarn pre-installed, which is not ideal. Please refer to the [official node image documentation](https://hub.docker.com/_/node/) for more information.

## Prerequisites

- Assumes you are using Yarn (you need to make some changes if you are using NPM)
- Assumes your build step can be triggered with `yarn build`
- Assumes you are using typescript and copies over `tsconfig.json`. You can remove this if you are not using TypeScript or this specific file.

There are very few other prerequisites as this is a self-contained image template, but there are a few assumptions that you should be aware of. There are two specific places in the Dockerfile where a specific list of files is copied into the image when building. If your application uses any additional files or folders relevant either when building or when running the application, you will have to update these.

The reason I opted not to copy everything into the image upon building is to avoid possible scenarios where sensitive or (local) development specific files are accidentally copied in. It is safer to rely on a whitelist of sorts, as you'll know for sure what is being copied over.

### What files and folders are copied over

- ./src
- package.json
- yarn.lock
- tsconfig.json

## How to use this template

Clone or download the Dockerfile and add it to your application's repository. Then, open it up and customize it to fit your needs. It will mostly work out of the box, but you might have to add your own environment variables or the like in. Look for the line comment in the Dockerfile to see where you can best do that.

## Contribution

Do you have any recommendations, suggestions or feedback? Please feel free to submit pull requests or reach out ot me over at [Twitter](https://twitter.com/hellodeibu).

I hope this is of some use to you.

Thank you kindly!
