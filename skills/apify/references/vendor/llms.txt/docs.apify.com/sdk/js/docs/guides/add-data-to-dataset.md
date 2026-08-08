# Add data to dataset

Copy for LLM

This example saves data to the default dataset. If the dataset doesn't exist, it will be created. You can save data to custom datasets by using [`Actor.openDataset()`](https://docs.apify.com/sdk/js/sdk/js/reference/class/Dataset.md#open)

[Run on](https://console.apify.com/actors/kk67IcZkKSSBTslXI?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCB7IEFjdG9yIH0gZnJvbSAnYXBpZnknO1xcbmltcG9ydCB7IENoZWVyaW9DcmF3bGVyIH0gZnJvbSAnY3Jhd2xlZSc7XFxuXFxuYXdhaXQgQWN0b3IuaW5pdCgpO1xcblxcbi8vIENyZWF0ZSBhIGRhdGFzZXQgd2hlcmUgd2Ugd2lsbCBzdG9yZSB0aGUgcmVzdWx0cy5cXG5jb25zdCBjcmF3bGVyID0gbmV3IENoZWVyaW9DcmF3bGVyKHtcXG4gICAgLy8gRnVuY3Rpb24gY2FsbGVkIGZvciBlYWNoIFVSTFxcbiAgICBhc3luYyByZXF1ZXN0SGFuZGxlcih7IHJlcXVlc3QsIGJvZHkgfSkge1xcbiAgICAgICAgLy8gU2F2ZSBkYXRhIHRvIGRlZmF1bHQgZGF0YXNldFxcbiAgICAgICAgYXdhaXQgQWN0b3IucHVzaERhdGEoe1xcbiAgICAgICAgICAgIHVybDogcmVxdWVzdC51cmwsXFxuICAgICAgICAgICAgaHRtbDogYm9keSxcXG4gICAgICAgIH0pO1xcbiAgICB9LFxcbn0pO1xcblxcbi8vIFJ1biB0aGUgY3Jhd2xlclxcbmF3YWl0IGNyYXdsZXIucnVuKFtcXG4gICAgeyB1cmw6ICdodHRwOi8vd3d3LmV4YW1wbGUuY29tL3BhZ2UtMScgfSxcXG4gICAgeyB1cmw6ICdodHRwOi8vd3d3LmV4YW1wbGUuY29tL3BhZ2UtMicgfSxcXG4gICAgeyB1cmw6ICdodHRwOi8vd3d3LmV4YW1wbGUuY29tL3BhZ2UtMycgfSxcXG5dKTtcXG5cXG5hd2FpdCBBY3Rvci5leGl0KCk7XFxuXCJ9Iiwib3B0aW9ucyI6eyJidWlsZCI6ImxhdGVzdCIsImNvbnRlbnRUeXBlIjoiYXBwbGljYXRpb24vanNvbjsgY2hhcnNldD11dGYtOCIsIm1lbW9yeSI6MTAyNCwidGltZW91dCI6MTgwfX0.fhpAfqCjjEMd7THx-jtJurjuRe7si1RztaBrOcDRcQ8\&asrc=run_on_apify)

```
import { Actor } from 'apify';

import { CheerioCrawler } from 'crawlee';



await Actor.init();



// Create a dataset where we will store the results.

const crawler = new CheerioCrawler({

    // Function called for each URL

    async requestHandler({ request, body }) {

        // Save data to default dataset

        await Actor.pushData({

            url: request.url,

            html: body,

        });

    },

});



// Run the crawler

await crawler.run([

    { url: 'http://www.example.com/page-1' },

    { url: 'http://www.example.com/page-2' },

    { url: 'http://www.example.com/page-3' },

]);



await Actor.exit();
```

Each item in this dataset will be saved to its own file in the following directory:

```
{PROJECT_FOLDER}/storage/datasets/default/
```
