- 👋 Hi, I’m @Hackersrlosers
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
Hackersrlosers/Hackersrlosers is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
Ive been hacked and wanna know what this shit is.
// pch.h in a Windows App SDK app
...
#include <shobjidl_core.h>
#include <microsoft.ui.xaml.window.h>
#include <winrt/Windows.ApplicationModel.DataTransfer.h>
...

// MainWindow.xaml.cpp
...
void MainWindow::myButton_Click(IInspectable const&, RoutedEventArgs const&)
{
    // Retrieve the window handle (HWND) of the current WinUI 3 window.
    auto windowNative{ this->try_as<::IWindowNative>() };
    winrt::check_bool(windowNative);
    HWND hWnd{ 0 };
    windowNative->get_WindowHandle(&hWnd);

    winrt::com_ptr<IDataTransferManagerInterop> interop = 
        winrt::get_activation_factory<Windows::ApplicationModel::DataTransfer::DataTransferManager,
        IDataTransferManagerInterop>();

    winrt::guid _dtm_iid{ 0xa5caee9b, 0x8708, 0x49d1, { 0x8d, 0x36, 0x67, 0xd2, 0x5a, 0x8d, 0xa0, 0x0c } };
    Windows::ApplicationModel::DataTransfer::DataTransferManager dataTransferManager{ nullptr };
    interop->GetForWindow(hWnd, _dtm_iid, winrt::put_abi(dataTransferManager));

    dataTransferManager.DataRequested([](Windows::ApplicationModel::DataTransfer::DataTransferManager const& /* sender */,
        Windows::ApplicationModel::DataTransfer::DataRequestedEventArgs const& args)
    {
        args.Request().Data().Properties().Title(L"In a desktop app...");
        args.Request().Data().SetText(L"...display WinRT UI objects that depend on CoreWindow.");
        args.Request().Data().RequestedOperation(Windows::ApplicationModel::DataTransfer::DataPackageOperation::Copy);
    });

    interop->ShowShareUIForWindow(hWnd);
}
...
