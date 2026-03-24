# Restore and Rebuild not working

## Problem

In case the restore/rebuild is not working, this might be related to the MSDN Subscription/License used by the colleague.

Known errors are

`Error occurred while restoring NuGet packages: Unable to load the service index for source https://pkgs.dev.azure.com/innersource/_packaging/DSS/nuget/v3/index.json`

![image.png](./_assets/image-a30bcfca-f465-4431-9f68-a95b3a3445ed.png)

## Solution

Open <https://innersource.visualstudio.com/DSS-Framework> and make sure you can see and access `Repos`

![image.png](./_assets/image-ad297495-3b55-4273-9250-f18524b47f20.png)

If this is not working/visible, navigate to <https://my.visualstudio.com/>` --> Subscriptions` and make sure you have at least Professional assigned. What will not work is `Dev Essentials`.

![image.png](./_assets/image-ff3c474d-7fff-4587-8f21-73a99c65bf89.png)

Align with your PM and use Self Service (https://avanade.service-now.com/) or the portal (https://avanade.sharepoint.com/sites/VisualStudioSubscriptions) to request the update.
