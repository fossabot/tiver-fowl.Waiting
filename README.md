# tiver-fowl.Waiting  [![MIT license](https://img.shields.io/badge/license-MIT-blue.svg)](https://raw.githubusercontent.com/MrHant/tiver-fowl/master/LICENSE)
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2FMrHant%2Ftiver-fowl.Waiting.svg?type=shield)](https://app.fossa.io/projects/git%2Bgithub.com%2FMrHant%2Ftiver-fowl.Waiting?ref=badge_shield)

"Wait" implementation.
Allows to process given condition until timeout is reached.
Overall timeout and polling interval are configurable.
Appearing exceptions can be ignored so processing of condition continues.

## Branch status

Branch | Package | Code style | CI
------ | ------- | ---------- | --
master (stable) | [![NuGet](https://img.shields.io/nuget/v/Tiver.Fowl.Waiting.svg)](https://www.nuget.org/packages/Tiver.Fowl.Waiting/) | [![Codacy Badge](https://api.codacy.com/project/badge/Grade/d62b7b7abc9d4aa9b5f3304b9e0f6af4?branch=master)](https://www.codacy.com/app/mr.hant/tiver-fowl.Waiting?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=MrHant/tiver-fowl.Waiting&amp;utm_campaign=Badge_Grade) | [![Build status](https://ci.appveyor.com/api/projects/status/eem0vm70l9o185qv/branch/master?svg=true)](https://ci.appveyor.com/project/MrHant/tiver-fowl-waiting/branch/master)
develop | [![NuGet Pre Release](https://img.shields.io/nuget/vpre/Tiver.Fowl.Waiting.svg)](https://www.nuget.org/packages/Tiver.Fowl.Waiting) | [![Codacy Badge](https://api.codacy.com/project/badge/Grade/d62b7b7abc9d4aa9b5f3304b9e0f6af4?branch=develop)](https://www.codacy.com/app/mr.hant/tiver-fowl.Waiting?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=MrHant/tiver-fowl.Waiting&amp;utm_campaign=Badge_Grade) | [![Build status](https://ci.appveyor.com/api/projects/status/eem0vm70l9o185qv/branch/develop?svg=true)](https://ci.appveyor.com/project/MrHant/tiver-fowl-waiting/branch/develop)

## Configurable

Can be configured via `App.config` file in following way:

    <!-- Configuration section-handler declaration area. -->
    <configSections>
        <sectionGroup name="waitConfigurationGroup">
            <section
                name="waitConfiguration"
                type="Tiver.Fowl.Waiting.Configuration.WaitConfigurationSection, Tiver.Fowl.Waiting"
                allowLocation="true"
                allowDefinition="Everywhere" />
        </sectionGroup>
    </configSections>

    <!-- Configuration section settings area. -->
    <waitConfigurationGroup>
        <waitConfiguration
            timeout="1000"
            pollingInterval="250" />
    </waitConfigurationGroup>


Full configuration can look like following:

    <waitConfiguration
        timeout="5000"
        pollingInterval="250"
        extendOnTimeout="true"
        extendedTimeout="15000" >
        <IgnoredExceptions>
            <Exception type="System.ArgumentException" />
            <Exception type="NUnit.Framework.AssertionException, NUnit.Framework" />
        </IgnoredExceptions>
    </waitConfiguration>

## Loggable

Produces debug log. Doesn't force any specific logging library. (uses LibLog v4.2.6)

You can transparently use any of following loggers:  NLog, Log4Net, EntLib Logging, Serilog and Loupe.

## Timeout Exception

Throws `Tiver.Fowl.Waiting.Exceptions.WaitTimeoutException` on timeout

## Ignoring  Exceptions

You can ignore exceptions during Wait

    // Following code throws System.DivideByZeroException
    var zero = 0;
    var wait = Wait.Until(() => 2 / zero);

    // Following code continue execution before timeout occurs
    var zero = 0;
    var wait = Wait.Until(() => 2 / zero, new WaitConfiguration(typeof(DivideByZeroException)));

## Samples

Simple Wait (use `App.config` values or defaults)

    var result = Wait.Until(() => 2 + 2);
    Assert.AreEqual(4, result);

Simple Wait with specific config

    var config = new WaitConfiguration(1000, 250);
    var result = Wait.Until(() => 2 + 2, config);
    Assert.AreEqual(4, result);

Extensible Wait

    var config = new WaitConfiguration(1000, 250, 5000);
    var result = Wait.Until(() => 2 + 2, config);
    Assert.AreEqual(4, result);


## License
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2FMrHant%2Ftiver-fowl.Waiting.svg?type=large)](https://app.fossa.io/projects/git%2Bgithub.com%2FMrHant%2Ftiver-fowl.Waiting?ref=badge_large)