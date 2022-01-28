[TOC]

# Performance Tests

## Benchmark Macros

BV TestFramework defines some benchmarking macros to define necessary properties.
Each macro:

- Uses ReportAggregatesOnly( true ) flag
- Defines max and min value statistic computing function
- Defines time unit as milliseconds

You should always use defined macros, because further processing steps depend on format of content of output files.

Macro name | Function
-----------|-----------------------
BV_BENCHMARK | Basic BV macro wrapping Google Benchmark macro and adding basic required properties.
MAINLOOP_BENCHMARK | Macro for main loop performance. User should write only scenes initialization code.


## Micro-benchmarks

Micro-benchmarks are usufull for testing small functionalities which don't need entire engine context to work.
These benchmarks use only GoogleBenchmark testing framework without TestFramework.
Check documentation on https://github.com/google/benchmark

### Creating project

You can use predefined template for micro-benchmark which is placed in **BlackVision\Dep\VisualStudioTemplates\Projects\Win\VS14\DefaultBenchmark**.
Copy whole directory, rename projects and add everything to solution.

### Writing benchmarks

Here is example of simple benchmark.

```
#!c++


#include "Framework/BVBenchmark.h"


// ***********************
//
static void         BM_15ParamsEvaluation       ( benchmark::State& state )
{
    /// Initialize objects meassure performance.
    auto timeline = OffsetTimeEvaluator::Create( "Timeline", 0.0f );

    std::vector< DefaultPluginParamValModelPtr > models;
    
    models.resize( state.range( 0 ) );
    for( auto & model : models )
        model = CreateSingleModel( timeline );

    // Only things inside this loop will be meassured.
    for( auto _ : state )
    {
        timeline->SetGlobalTime( 1.0f );

        for( auto & model : models )
            model->Update();
    }
}

BV_BENCHMARK( BM_15ParamsEvaluation )->Arg( 200 )->Arg( 1500 )->Repetitions( 30 );
```

## Framework benchmarks

Framework benchmarks can meassure main loop performance.

### Creating project

You can use predefined template for framework benchmark which is placed in **BlackVision\Dep\VisualStudioTemplates\Projects\Win\VS14\FrameworkBenchmark**.
Copy whole directory, rename projects and add everything to solution.

### Benchmark example

MAINLOOP_BENCHMARK macro implement all necessary things to call main loop and unload all scenes after benchamrk is finished. All you have to do is to implment initialization code.


```
#!c++

// ***********************
//
void            SceneBuffersOverhead  ( benchmark::State& state )
{
    auto env = TestEnvironment::GetEnvironment();

    auto editor = env->GetAppLogic()->GetBVProject()->GetProjectEditor();
    CreateEmptyScenes( editor, state.range( 0 ) );
}


MAINLOOP_BENCHMARK( SceneBuffersOverhead )->Arg( 30 )->Iterations( 2000 )->Repetitions( 20 );
```



## Run all benchmarks

To run all benchmarks invoke batch script RunBenchmarks.bat


```
#!c++

./RunBenchmarks.bat x64 Release v140 Reports/Benchmarks/
```

This will produce output csv files in **Reports/Benchmarks/** directory.

### Automatic benchmarks

CI Server will automatically execute all benchmarks after each build. You can check results in **Plots**. Server runs only one build at the same time. Otherwise benchmarks meassurments would be affected. We must solve this problem in future.