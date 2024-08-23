# Parallel Rotate Images C#
Hello, this is a program mainly testing the parallel feature of C#. Here is the relatively short Program.cs code:
```

// This project demonstrates how to rotate multiple images in parallel using Aspose.Imaging for .NET.
 
using System;
using System.IO;
using System.Diagnostics;
using Aspose.Imaging;
class Program
{
    static void Main(string[] args)
    {
        // Prompt user for load directory containing images to rotate, check that it exists
        string? loadPath;
        Console.Write("Enter folder name found in bin\\Debug\\net8.0 (leave empty for default): ");
        loadPath = Console.ReadLine();
        if (string.IsNullOrEmpty(loadPath))
        {
            loadPath = @"pictures\";
        }
        if (!Directory.Exists(loadPath))
        {
            Console.WriteLine($"Could not find directory {loadPath}");
            return;
        }
        Console.WriteLine($"Loading images from: {loadPath}");

        // Prompt user for save directory path, create it if it doesn't exist
        string? savePath;
        Console.Write("Enter new directory path (leave empty for default): ");
        savePath = Console.ReadLine();
        if(string.IsNullOrEmpty(savePath))
        {
            savePath = @"Rotated Pictures\";
        }
        DirectoryInfo myDirInfo = Directory.CreateDirectory(savePath);
        // Test Write: Console.WriteLine($"Directory Created: {myDirInfo.ToString()}");

        // Read each image from load directory, flip it, and save it into the save directory
        var files = Directory.GetFiles(loadPath);
        Stopwatch stopwatch = new Stopwatch();
        stopwatch.Start();
        Parallel.ForEach(files, file =>
        {
            // Load image
            Console.WriteLine($"Using Thread: {Thread.CurrentThread.ManagedThreadId}");
            Image image = Image.Load(file);
            if (image == null)
            {
                Console.WriteLine($"Error loading image from file: {file}");
                return;
            }

            // Rotate image
            try
            {
                image.RotateFlip(RotateFlipType.Rotate90FlipNone);
            }
            catch (Exception e)
            {
                Console.WriteLine($"Error rotating image: {e.Message}");
                return;
            }

            // Save image in new directory
            image.Save(savePath + "\\" + file.Substring(loadPath.Length));
        });
        stopwatch.Stop();
        Console.WriteLine("Rotated files saved to: " + savePath);
        Console.WriteLine($"ParallelExecution time: {stopwatch.ToString()}");
        return;
    }
}

```
