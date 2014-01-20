# KJSEmbed

Binding Javascript object to QObjects

## Introduction

KSJEmbed Provides a method of binding JavaScript objects to QObjects,
so you can script your applications.


## Usage

If you are using CMake, you need to have

    find_package(KF5JsEmbed NO_MODULE)

(or similar) in your CMakeLists.txt file, and you need to link to KF5::JsEmbed.

The KJSEmbed::Engine class provides the main interface for running embedded
Javascript.

    KJSEmbed::Engine *engine = new KJSEmbed::Engine();
    KJS::Interpreter *interpreter = engine->interpreter();
    interpreter->setShouldPrintExceptions(true);
    KJS::ExecState *exec = interpreter->globalExec();
    KJS::UString code("print(\"Hello World\")");
    KJSEmbed::Engine::ExitStatus exitstatus = engine->execute(code);
    KJS::Completion completion = engine->completion();
    if (exitstatus != KJSEmbed::Engine::Success) {
        KJS::JSValue* value = completion.value();
        qDebug() << value->toString(exec).qstring();
    }


## Links

- Home page: <https://projects.kde.org/projects/frameworks/kjsembed>
- Mailing list: <https://mail.kde.org/mailman/listinfo/kde-frameworks-devel>
- IRC channel: #kde-devel on Freenode
- Git repository: <https://projects.kde.org/projects/frameworks/kjsembed/repository>
